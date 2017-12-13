---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: "Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: Data formuláře urlencoded | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0ed339c4f9d5854ab5a21cdd077a4d494987101f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="2e119-102">Odeslání dat formuláře HTML v rozhraní ASP.NET Web API: Data formuláře urlencoded</span><span class="sxs-lookup"><span data-stu-id="2e119-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="2e119-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2e119-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="2e119-104">Část 1: Data formuláře urlencoded</span><span class="sxs-lookup"><span data-stu-id="2e119-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="2e119-105">Tento článek ukazuje, jak k odesílání dat formuláře urlencoded na kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2e119-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="2e119-106">Přehled formuláře HTML</span><span class="sxs-lookup"><span data-stu-id="2e119-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="2e119-107">Odesílání komplexních typů</span><span class="sxs-lookup"><span data-stu-id="2e119-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="2e119-108">Odesílání dat formuláře pomocí rozhraní AJAX</span><span class="sxs-lookup"><span data-stu-id="2e119-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="2e119-109">Odesílání jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="2e119-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="2e119-110">[Stáhněte si dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="2e119-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="2e119-111">Přehled formuláře HTML</span><span class="sxs-lookup"><span data-stu-id="2e119-111">Overview of HTML Forms</span></span>

<span data-ttu-id="2e119-112">Použití formuláře HTML GET nebo POST k odesílání dat na server.</span><span class="sxs-lookup"><span data-stu-id="2e119-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="2e119-113">**Metoda** atribut **formuláře** element poskytuje metodu protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="2e119-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="2e119-114">Výchozí metoda je GET.</span><span class="sxs-lookup"><span data-stu-id="2e119-114">The default method is GET.</span></span> <span data-ttu-id="2e119-115">Pokud formulář používá získáte, formulář, který je zakódován data v identifikátoru URI jako řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="2e119-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="2e119-116">Pokud formulář používá POST, data formuláře je umístěn v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e119-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="2e119-117">Pro data účtováno **enctype** Určuje atribut formátu textu na žádost:</span><span class="sxs-lookup"><span data-stu-id="2e119-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="2e119-118">enctype</span><span class="sxs-lookup"><span data-stu-id="2e119-118">enctype</span></span> | <span data-ttu-id="2e119-119">Popis</span><span class="sxs-lookup"><span data-stu-id="2e119-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2e119-120">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="2e119-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="2e119-121">Data formuláře je kódovaná jako dvojice název/hodnota, podobně jako řetězec dotazu identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="2e119-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="2e119-122">Toto je výchozí formát pro metodu POST.</span><span class="sxs-lookup"><span data-stu-id="2e119-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="2e119-123">multipart/formulář data</span><span class="sxs-lookup"><span data-stu-id="2e119-123">multipart/form-data</span></span> | <span data-ttu-id="2e119-124">Data formuláře je kódovaná jako vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="2e119-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="2e119-125">Pokud odesíláte soubor na server, použijte tento formát.</span><span class="sxs-lookup"><span data-stu-id="2e119-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="2e119-126">Část 1 / Tento článek vypadá na formát x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="2e119-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="2e119-127">[Část 2](sending-html-form-data-part-2.md) popisuje vícedílné zprávy standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="2e119-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="2e119-128">Odesílání komplexních typů</span><span class="sxs-lookup"><span data-stu-id="2e119-128">Sending Complex Types</span></span>

<span data-ttu-id="2e119-129">Obvykle se odesílají komplexního typu, skládá z hodnot, které jsou převzaty z několika ovládacích prvků formuláře.</span><span class="sxs-lookup"><span data-stu-id="2e119-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="2e119-130">Vezměte v úvahu následující model, který představuje stav aktualizace:</span><span class="sxs-lookup"><span data-stu-id="2e119-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="2e119-131">Tady je kontroleru webového rozhraní API, který přijímá `Update` objektu přes POST.</span><span class="sxs-lookup"><span data-stu-id="2e119-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="2e119-132">Tento řadič používá [směrování podle akce](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), takže je šablona trasy &quot;rozhraní api nebo {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="2e119-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="2e119-133">Klient bude odeslána data, která mají &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="2e119-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="2e119-134">Teď můžete napsat formuláře HTML pro uživatele k odeslání aktualizace stavu.</span><span class="sxs-lookup"><span data-stu-id="2e119-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="2e119-135">Všimněte si, že **akce** atribut na formuláři je identifikátor URI naše akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="2e119-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="2e119-136">Tady je formulář s některé hodnoty zadané v:</span><span class="sxs-lookup"><span data-stu-id="2e119-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="2e119-137">Po kliknutí na odeslání, prohlížeč odešle požadavek HTTP podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="2e119-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="2e119-138">Všimněte si, že text žádosti obsahuje data formuláře, naformátovaná jako dvojice název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="2e119-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="2e119-139">Webové rozhraní API automaticky převede dvojice název/hodnota do instance `Update` třídy.</span><span class="sxs-lookup"><span data-stu-id="2e119-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="2e119-140">Odesílání dat formuláře pomocí rozhraní AJAX</span><span class="sxs-lookup"><span data-stu-id="2e119-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="2e119-141">Když uživatel odešle formulář, prohlížeč přejde mimo aktuální stránku a vykreslí tělo zprávy s odpovědí.</span><span class="sxs-lookup"><span data-stu-id="2e119-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="2e119-142">Který je v pořádku. Pokud je odpověď na stránce HTML.</span><span class="sxs-lookup"><span data-stu-id="2e119-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="2e119-143">Pomocí webového rozhraní API, ale text odpovědi je obvykle buď prázdná, nebo obsahuje strukturovaných dat, jako je například JSON.</span><span class="sxs-lookup"><span data-stu-id="2e119-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="2e119-144">V takovém případě je vhodnější pro odeslání žádosti o data formuláře pomocí AJAX, tak, aby stránky může zpracovat odpověď.</span><span class="sxs-lookup"><span data-stu-id="2e119-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="2e119-145">Následující kód ukazuje, jak vystavit data formuláře pomocí jQuery.</span><span class="sxs-lookup"><span data-stu-id="2e119-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="2e119-146">JQuery **odeslání** funkce nahrazuje akce formuláře s novou funkci.</span><span class="sxs-lookup"><span data-stu-id="2e119-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="2e119-147">Přepíše výchozí chování tlačítka pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="2e119-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="2e119-148">**Serializovat** funkce serializuje data formuláře do dvojice název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="2e119-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="2e119-149">Chcete-li odeslat data formuláře do serveru, volejte `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="2e119-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="2e119-150">Po dokončení žádosti `.success()` nebo `.error()` obslužná rutina zobrazí odpovídající zprávu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="2e119-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="2e119-151">Odesílání jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="2e119-151">Sending Simple Types</span></span>

<span data-ttu-id="2e119-152">V předchozích částech jsme vám poslali komplexní typ, který webového rozhraní API deserializovat do instance třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="2e119-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="2e119-153">Můžete také odeslat jednoduché typy, jako je například řetězec.</span><span class="sxs-lookup"><span data-stu-id="2e119-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="2e119-154">Před odesláním jednoduchého typu, zvažte, místo toho zabalení hodnota v komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="2e119-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="2e119-155">To vám dává výhod modelu ověření na straně serveru a usnadňuje rozšířit modelu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="2e119-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="2e119-156">Základní postup odesílání jednoduchý typ. jsou stejné, ale existují dva drobné rozdíly.</span><span class="sxs-lookup"><span data-stu-id="2e119-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="2e119-157">V kontroleru, musíte nejdřív uspořádání název parametru se **FromBody** atribut.</span><span class="sxs-lookup"><span data-stu-id="2e119-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="2e119-158">Ve výchozím nastavení se webové rozhraní API pokusí získat jednoduché typy z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e119-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="2e119-159">**FromBody** atribut informuje webového rozhraní API načíst hodnotu z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e119-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="2e119-160">Webové rozhraní API přečte text odpovědi alespoň jednou, takže jenom jeden parametr akce mohou pocházet z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="2e119-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="2e119-161">Pokud potřebujete získat více hodnot z textu žádosti, definujte komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="2e119-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="2e119-162">Druhý klient musí odeslat hodnotu v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="2e119-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="2e119-163">Část názvu dvojici název – hodnota musí být konkrétně pro jednoduchý typ. prázdné.</span><span class="sxs-lookup"><span data-stu-id="2e119-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="2e119-164">Některé prohlížeče podporují formuláře HTML, ale můžete vytvořit tento formát ve skriptu takto:</span><span class="sxs-lookup"><span data-stu-id="2e119-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="2e119-165">Tady je příklad formuláře:</span><span class="sxs-lookup"><span data-stu-id="2e119-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="2e119-166">A tady je skript, který chcete odeslat hodnot formulářů.</span><span class="sxs-lookup"><span data-stu-id="2e119-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="2e119-167">Jediným rozdílem z předchozí skript je argument předaný do **post** funkce.</span><span class="sxs-lookup"><span data-stu-id="2e119-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="2e119-168">Můžete použít ve stejný přístup k odeslání pole jednoduchých typů:</span><span class="sxs-lookup"><span data-stu-id="2e119-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="2e119-169">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="2e119-169">Additional Resources</span></span>

[<span data-ttu-id="2e119-170">Část 2: Odeslání souboru a vícedílné zprávy standardu MIME</span><span class="sxs-lookup"><span data-stu-id="2e119-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
