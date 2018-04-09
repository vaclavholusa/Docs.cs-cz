---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Ověřování uživatelského vstupu v rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje, jak ověřit informace získáte od uživatelů &mdash; to znamená, abyste měli jistotu, že uživatelé zadat platné informace ve formátu HTML forms v přihlašovacím...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="05474-103">Ověřování uživatelského vstupu v lokalitách rozhraní ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="05474-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="05474-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="05474-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="05474-105">Tento článek popisuje, jak ověřit informace získáte od uživatelů &mdash; to znamená, abyste měli jistotu, že uživatelé zadat platné informace ve formátu HTML forms v stránku ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="05474-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="05474-106">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="05474-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="05474-107">Jak zkontrolovat, že vstup uživatele odpovídá ověřovací kritéria, které definujete.</span><span class="sxs-lookup"><span data-stu-id="05474-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="05474-108">Jak určit, zda byly úspěšně všechny testy pro ověření.</span><span class="sxs-lookup"><span data-stu-id="05474-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="05474-109">Postupy: zobrazení chyb při ověřování (a způsob jejich formátování).</span><span class="sxs-lookup"><span data-stu-id="05474-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="05474-110">Jak ověřit data, která nepřejde do stavu přímo od uživatelů.</span><span class="sxs-lookup"><span data-stu-id="05474-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="05474-111">Jsou to ASP.NET programování koncepty představené v článku:</span><span class="sxs-lookup"><span data-stu-id="05474-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="05474-112">`Validation` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="05474-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="05474-113">`Html.ValidationSummary` a `Html.ValidationMessage` metody.</span><span class="sxs-lookup"><span data-stu-id="05474-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="05474-114">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="05474-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="05474-115">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="05474-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="05474-116">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="05474-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="05474-117">Tento článek obsahuje následující části:</span><span class="sxs-lookup"><span data-stu-id="05474-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="05474-118">Přehled ověřování uživatelského vstupu</span><span class="sxs-lookup"><span data-stu-id="05474-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="05474-119">Ověřování uživatelského vstupu</span><span class="sxs-lookup"><span data-stu-id="05474-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="05474-120">Přidání ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="05474-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="05474-121">Formátování chyby ověření</span><span class="sxs-lookup"><span data-stu-id="05474-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="05474-122">Ověřování dat, která nepřejde do stavu přímo od uživatelů</span><span class="sxs-lookup"><span data-stu-id="05474-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="05474-123">Přehled ověřování uživatelského vstupu</span><span class="sxs-lookup"><span data-stu-id="05474-123">Overview of User Input Validation</span></span>

<span data-ttu-id="05474-124">Pokud se požádat uživatele k zadání informací na stránce – například do formuláře – je důležité se ujistit, zda jsou hodnoty, které jsou zadány platné.</span><span class="sxs-lookup"><span data-stu-id="05474-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="05474-125">Například nechcete zpracovat formulář, který chybí důležitých informací.</span><span class="sxs-lookup"><span data-stu-id="05474-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="05474-126">Při zadávání hodnot do formuláře HTML, jsou hodnoty, které budou zadejte řetězce.</span><span class="sxs-lookup"><span data-stu-id="05474-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="05474-127">V mnoha případech jsou hodnoty, které budete potřebovat některé jiné datové typy, jako celá čísla nebo kalendářní data.</span><span class="sxs-lookup"><span data-stu-id="05474-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="05474-128">Proto máte také zajistit, že hodnoty, které uživatelé zadají lze správně převést na příslušné datové typy.</span><span class="sxs-lookup"><span data-stu-id="05474-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="05474-129">Také může mít určitá omezení hodnot.</span><span class="sxs-lookup"><span data-stu-id="05474-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="05474-130">I když uživatelé správně zadejte celé číslo, například může musíte zajistit, že hodnota spadá do určitého rozsahu.</span><span class="sxs-lookup"><span data-stu-id="05474-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![Chyby ověření, které používají třídy CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="05474-132">**Důležité** ověřování uživatelského vstupu je také důležité pro zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="05474-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="05474-133">Pokud omezíte hodnoty, které mohou uživatelé zadat ve formulářích, se snižuje riziko, že někdo můžete zadat hodnotu, která může ohrozit zabezpečení vašeho webu.</span><span class="sxs-lookup"><span data-stu-id="05474-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="05474-134">Ověřování uživatelského vstupu</span><span class="sxs-lookup"><span data-stu-id="05474-134">Validating User Input</span></span>

<span data-ttu-id="05474-135">V technologii ASP.NET Web Pages 2, můžete použít `Validator` pomocná rutina pro testovací uživatelský vstup.</span><span class="sxs-lookup"><span data-stu-id="05474-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="05474-136">Základní postup je provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="05474-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="05474-137">Určete, které vstupní elementy (pole), kterou chcete ověřit.</span><span class="sxs-lookup"><span data-stu-id="05474-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="05474-138">Obvykle ověření hodnot v `<input>` prvky ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="05474-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="05474-139">Je však vhodné ověřit veškerý vstup, vstupní i který přichází z omezené element jako `<select>` seznamu.</span><span class="sxs-lookup"><span data-stu-id="05474-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="05474-140">To pomáhá zajistit, že nemáte uživatelům obejít ovládací prvky na stránce a odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="05474-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="05474-141">V kódu stránky přidat jednotlivé ověřovací kontroly pro každý vstupní element pomocí metody `Validation` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="05474-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="05474-142">Chcete-li vyhledat povinná pole, použijte `Validation.RequireField(field, [error message])` (pro jednotlivá pole) nebo `Validation.RequireFields(field1, field2, ...))` (pro seznam polí).</span><span class="sxs-lookup"><span data-stu-id="05474-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="05474-143">Pro jiné typy ověřování, použijte `Validation.Add(field, ValidationType)`.</span><span class="sxs-lookup"><span data-stu-id="05474-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="05474-144">Pro `ValidationType`, můžete použít tyto možnosti:</span><span class="sxs-lookup"><span data-stu-id="05474-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. <span data-ttu-id="05474-145">Při odeslání stránky, zkontrolujte, zda byla ověření úspěšná kontrolou `Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="05474-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="05474-146">Pokud nejsou žádné chyby ověření, můžete přeskočit zpracování normální stránky.</span><span class="sxs-lookup"><span data-stu-id="05474-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="05474-147">Například pokud účelem stránky je aktualizace databáze, můžete si udělat dokud byly opraveny všechny chyby ověřování.</span><span class="sxs-lookup"><span data-stu-id="05474-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="05474-148">Pokud nejsou chyby ověření, zobrazení chybové zprávy v značek pomocí `Html.ValidationSummary` nebo `Html.ValidationMessage`, nebo obojí.</span><span class="sxs-lookup"><span data-stu-id="05474-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="05474-149">Následující příklad ukazuje na stránku, který znázorňuje tyto kroky.</span><span class="sxs-lookup"><span data-stu-id="05474-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="05474-150">Pokud chcete zjistit, jak funguje ověření, spusťte tuto stránku a úmyslně uděláte chyby.</span><span class="sxs-lookup"><span data-stu-id="05474-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="05474-151">Zde je například vzhled stránky, pokud zapomenete zadat název postupu, pokud zadáte došlo, a při zadání neplatná data:</span><span class="sxs-lookup"><span data-stu-id="05474-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![Chyby ověření na vykreslené stránce](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="05474-153">Přidání ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="05474-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="05474-154">Ve výchozím nastavení, uživatelský vstup je ověřen po odeslání stránky – to znamená, ověření se provádí v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="05474-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="05474-155">Nevýhodou tohoto přístupu je, že uživatelé neznáte, že udělali chybu až po jejich odeslání stránky.</span><span class="sxs-lookup"><span data-stu-id="05474-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="05474-156">Pokud formulář je dlouhá nebo komplexní, může být nepraktické uživatele zasílání zpráv o chybách, až po odeslání stránky.</span><span class="sxs-lookup"><span data-stu-id="05474-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="05474-157">Můžete přidat podporu k provedení ověření v klientského skriptu.</span><span class="sxs-lookup"><span data-stu-id="05474-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="05474-158">V takovém případě ověření se provádí, jak uživatelé pracují v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="05474-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="05474-159">Předpokládejme například, že zadáte, hodnota musí být celé číslo.</span><span class="sxs-lookup"><span data-stu-id="05474-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="05474-160">Pokud uživatel zadá hodnotu – celé číslo, chyba je uvádět, jakmile uživatel opustí vstupní pole.</span><span class="sxs-lookup"><span data-stu-id="05474-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="05474-161">Uživatelé získají okamžitou zpětnou vazbu, která je vhodné pro ně.</span><span class="sxs-lookup"><span data-stu-id="05474-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="05474-162">Ověřování na základě klienta může také snížit počet pokusů, které má uživatel k odeslání formuláře a opravte několik chyb.</span><span class="sxs-lookup"><span data-stu-id="05474-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="05474-163">I když používáte ověřování na straně klienta, ověření se provádí vždy také v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="05474-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="05474-164">Provedení ověření v serverovém kódu je bezpečnostní opatření, v případě, že uživatelé obejít ověřování na základě klienta.</span><span class="sxs-lookup"><span data-stu-id="05474-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="05474-165">Zaregistrujte následující knihovny jazyka JavaScript na stránce:</span><span class="sxs-lookup"><span data-stu-id="05474-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   <span data-ttu-id="05474-166">Dva knihoven jsou načíst z síti pro doručování obsahu (CDN), takže nemusíte nutně mít je na počítači nebo serveru.</span><span class="sxs-lookup"><span data-stu-id="05474-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="05474-167">Ale musí mít místní kopii *jquery.validate.unobtrusive.js*.</span><span class="sxs-lookup"><span data-stu-id="05474-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="05474-168">Pokud dosud nepracujete pomocí služby WebMatrix šablony (jako je **Starter Site** ), který obsahuje knihovny, vytvořit web webové stránky, který je založen na **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="05474-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="05474-169">Zkopírujte *.js* souboru k aktuální lokalitě.</span><span class="sxs-lookup"><span data-stu-id="05474-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="05474-170">V kód pro každý element, který jste se ověřování, že přidáte volání `Validation.For(field)`.</span><span class="sxs-lookup"><span data-stu-id="05474-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="05474-171">Tato metoda vysílá atributy, které používají ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="05474-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="05474-172">(Místo generování skutečný kód jazyka JavaScript, metoda vysílá atributů, například `data-val-...`.</span><span class="sxs-lookup"><span data-stu-id="05474-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="05474-173">Ověření nerušivého klienta, který používá jQuery pro práci podporu těchto atributů.)</span><span class="sxs-lookup"><span data-stu-id="05474-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="05474-174">Na následující stránce ukazuje, jak přidat funkce ověření klienta v příkladu uvedena výše.</span><span class="sxs-lookup"><span data-stu-id="05474-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="05474-175">Ne všechny ověřovací kontroly spouští na klientovi.</span><span class="sxs-lookup"><span data-stu-id="05474-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="05474-176">Ověření typu dat (celé číslo, datum a tak dále) na konkrétní nespouštět na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="05474-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="05474-177">Následující kontroly fungovat na klientovi a serveru:</span><span class="sxs-lookup"><span data-stu-id="05474-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="05474-178">V tomto příkladu nebude fungovat test pro platné datum v kódu klienta.</span><span class="sxs-lookup"><span data-stu-id="05474-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="05474-179">Test se však provést v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="05474-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="05474-180">Formátování chyby ověření</span><span class="sxs-lookup"><span data-stu-id="05474-180">Formatting Validation Errors</span></span>

<span data-ttu-id="05474-181">Můžete řídit, jak se zobrazí chyby ověření definováním třídy CSS, které mají následující názvy vyhrazené:</span><span class="sxs-lookup"><span data-stu-id="05474-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="05474-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="05474-182">`field-validation-error`.</span></span> <span data-ttu-id="05474-183">Definuje výstup `Html.ValidationMessage` metoda při zobrazuje chybu.</span><span class="sxs-lookup"><span data-stu-id="05474-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="05474-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="05474-184">`field-validation-valid`.</span></span> <span data-ttu-id="05474-185">Definuje výstup `Html.ValidationMessage` v případě, že se nezobrazí žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="05474-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="05474-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="05474-186">`input-validation-error`.</span></span> <span data-ttu-id="05474-187">Definuje jak `<input>` elementy vykresleny, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="05474-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="05474-188">(Například tuto třídu můžete použít k nastavení barvy pozadí &lt;vstupní&gt; element na jinou barvu, pokud je jeho hodnota je neplatná.) Tato třída šablon stylů CSS se používá pouze během ověření klienta (v ASP.NET Web Pages 2).</span><span class="sxs-lookup"><span data-stu-id="05474-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="05474-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="05474-189">`input-validation-valid`.</span></span> <span data-ttu-id="05474-190">Definuje vzhled `<input>` prvky, pokud se nezobrazí žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="05474-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="05474-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="05474-191">`validation-summary-errors`.</span></span> <span data-ttu-id="05474-192">Definuje výstup `Html.ValidationSummary` metoda zobrazit seznam chyb.</span><span class="sxs-lookup"><span data-stu-id="05474-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="05474-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="05474-193">`validation-summary-valid`.</span></span> <span data-ttu-id="05474-194">Definuje výstup `Html.ValidationSummary` v případě, že se nezobrazí žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="05474-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="05474-195">Následující `<style>` bloku jsou pravidla pro chybové podmínky.</span><span class="sxs-lookup"><span data-stu-id="05474-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="05474-196">Pokud zahrnete tohoto bloku stylu příklad stránky z dříve v článku, zobrazení chyb bude vypadat jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="05474-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![Chyby ověření, které používají třídy CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="05474-198">Pokud nepoužíváte ověření klienta v ASP.NET Web Pages 2, pro třídy CSS `<input>` elementy (`input-validation-error` a `input-validation-valid` nemají žádný vliv.</span><span class="sxs-lookup"><span data-stu-id="05474-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="05474-199">Statické a dynamické chybových zpráv</span><span class="sxs-lookup"><span data-stu-id="05474-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="05474-200">Pravidla šablon stylů CSS vstoupí v párech, jako například `validation-summary-errors` a `validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="05474-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="05474-201">Těmto párům umožňují definovat pravidla pro obě podmínky: chybový stav a podmínku "normální" (bez chyb).</span><span class="sxs-lookup"><span data-stu-id="05474-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="05474-202">Je důležité si uvědomit, že vždy vykreslení značky pro zobrazení chyb, i když nejsou žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="05474-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="05474-203">Například, pokud stránka nemá `Html.ValidationSummary` metoda do kódu stránky zdroj bude obsahovat následující kód, i v případě, že při prvním požadavku na stránku:</span><span class="sxs-lookup"><span data-stu-id="05474-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="05474-204">Jinými slovy `Html.ValidationSummary` vždy vykreslí metoda `<div>` elementu a seznam, i když v seznamu chyb je prázdný.</span><span class="sxs-lookup"><span data-stu-id="05474-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="05474-205">Podobně `Html.ValidationMessage` vždy vykreslí metoda `<span>` element jako zástupný symbol pro chybu jednotlivá pole, i když se nezobrazí žádná chyba.</span><span class="sxs-lookup"><span data-stu-id="05474-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="05474-206">V některých situacích s chybovou zprávou, může způsobit stránky přeformátování a může způsobit elementy na stránce pohyb.</span><span class="sxs-lookup"><span data-stu-id="05474-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="05474-207">Pravidla šablon stylů CSS, která končit `-valid` umožňují definovat rozložení, které pomáhají zabránit tento problém.</span><span class="sxs-lookup"><span data-stu-id="05474-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="05474-208">Například můžete definovat `field-validation-error` a `field-validation-valid` zároveň mít stejné pevnou velikost.</span><span class="sxs-lookup"><span data-stu-id="05474-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="05474-209">Tímto způsobem oblasti zobrazení pro toto pole je statická a tok stránky se nezmění, pokud se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="05474-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="05474-210">Ověřování dat, která nepřejde do stavu přímo od uživatelů</span><span class="sxs-lookup"><span data-stu-id="05474-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="05474-211">Někdy je nutné ověřit informace, které nepřejde do stavu přímo z formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="05474-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="05474-212">Typickým příkladem je stránka, kde je hodnota předaná řetězce dotazu, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="05474-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="05474-213">V takovém případě chcete Ujistěte se, že hodnota, která je předána na stránku (tady 1022 pro hodnotu `classid`) je platný.</span><span class="sxs-lookup"><span data-stu-id="05474-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="05474-214">Nelze použít přímo `Validation` pomocná rutina pro toto ověření proveďte.</span><span class="sxs-lookup"><span data-stu-id="05474-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="05474-215">Můžete však použít jiné funkce ověřování systému, jako je možnost zobrazit chybové zprávy ověření.</span><span class="sxs-lookup"><span data-stu-id="05474-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="05474-216">**Důležité** vždy ověřte hodnoty, které jste získali z *žádné* zdroje, včetně hodnot pole formuláře, hodnoty řetězce dotazu a hodnoty souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="05474-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="05474-217">Je snadné pro uživatele, kteří mají tyto hodnoty změnit (případně pro škodlivý účely).</span><span class="sxs-lookup"><span data-stu-id="05474-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="05474-218">Proto je nutné zkontrolovat tyto hodnoty ochráníte vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="05474-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="05474-219">Následující příklad ukazuje, jak může ověřit hodnotu, která je předán v řetězci dotazu.</span><span class="sxs-lookup"><span data-stu-id="05474-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="05474-220">Kód testuje, zda hodnota není prázdné a že je celé číslo.</span><span class="sxs-lookup"><span data-stu-id="05474-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="05474-221">Všimněte si, že test se provádí při žádost není odeslání formuláře (`if(!IsPost)`).</span><span class="sxs-lookup"><span data-stu-id="05474-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="05474-222">Tento test by úspěšně prošel zpracováním při prvním požadavku na stránku, ale není požadavek po odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="05474-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="05474-223">Chcete-li zobrazit tuto chybu, můžete přidat chyba do seznamu chyb ověřování voláním `Validation.AddFormError("message")`.</span><span class="sxs-lookup"><span data-stu-id="05474-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="05474-224">Pokud tato stránka obsahuje volání `Html.ValidationSummary` metoda, chyba se zobrazí existuje, stejně jako chyba ověření uživatelského vstupu.</span><span class="sxs-lookup"><span data-stu-id="05474-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="05474-225">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="05474-225">Additional Resources</span></span>

[<span data-ttu-id="05474-226">Práce s formuláře HTML na webech ASP.NET – webové stránky</span><span class="sxs-lookup"><span data-stu-id="05474-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
