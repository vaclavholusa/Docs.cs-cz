---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Posílání dat formulářů HTML ve webovém rozhraní API technologie ASP.NET: Data formuláře kódovaná | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2d01212cc408f8bb66fa3103464c9a1f7a1e21c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756390"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="26b61-102">Posílání dat formulářů HTML ve webovém rozhraní API technologie ASP.NET: Data formuláře kódovaná</span><span class="sxs-lookup"><span data-stu-id="26b61-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="26b61-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="26b61-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="26b61-104">– Část 1: Data formuláře kódovaná</span><span class="sxs-lookup"><span data-stu-id="26b61-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="26b61-105">Tento článek ukazuje, jak publikovat data formuláře kódovaná do kontroleru webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="26b61-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="26b61-106">Přehled formulářů HTML</span><span class="sxs-lookup"><span data-stu-id="26b61-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="26b61-107">Odesílání komplexních typů</span><span class="sxs-lookup"><span data-stu-id="26b61-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="26b61-108">Posílání dat formulářů prostřednictvím AJAX</span><span class="sxs-lookup"><span data-stu-id="26b61-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="26b61-109">Odesílání jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="26b61-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="26b61-110">[Stáhnout dokončený projekt](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="26b61-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="26b61-111">Přehled formulářů HTML</span><span class="sxs-lookup"><span data-stu-id="26b61-111">Overview of HTML Forms</span></span>

<span data-ttu-id="26b61-112">Používání formulářů HTML GET nebo POST k odesílání dat na server.</span><span class="sxs-lookup"><span data-stu-id="26b61-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="26b61-113">**Metoda** atribut **formuláře** element poskytuje metoda HTTP:</span><span class="sxs-lookup"><span data-stu-id="26b61-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="26b61-114">Výchozí metodou je GET.</span><span class="sxs-lookup"><span data-stu-id="26b61-114">The default method is GET.</span></span> <span data-ttu-id="26b61-115">Pokud formulář používá získáte, formuláře, který dat je zakódovaný v identifikátoru URI jako řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="26b61-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="26b61-116">Pokud formulář používá metodu POST, data formuláře je umístěn v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="26b61-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="26b61-117">Pro zaúčtované data **enctype** atribut určuje formát těla požadavku:</span><span class="sxs-lookup"><span data-stu-id="26b61-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="26b61-118">enctype</span><span class="sxs-lookup"><span data-stu-id="26b61-118">enctype</span></span> | <span data-ttu-id="26b61-119">Popis</span><span class="sxs-lookup"><span data-stu-id="26b61-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="26b61-120">Application/x--www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="26b61-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="26b61-121">Data formuláře kódovaná jako dvojice název/hodnota, podobně jako řetězce dotazu URI.</span><span class="sxs-lookup"><span data-stu-id="26b61-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="26b61-122">Toto je výchozí formát pro metodu POST.</span><span class="sxs-lookup"><span data-stu-id="26b61-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="26b61-123">multipart/formuláře data</span><span class="sxs-lookup"><span data-stu-id="26b61-123">multipart/form-data</span></span> | <span data-ttu-id="26b61-124">Data formuláře kódovaná jako vícedílné zprávě standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="26b61-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="26b61-125">Pokud nahráváte soubor na server, použijte tento formát.</span><span class="sxs-lookup"><span data-stu-id="26b61-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="26b61-126">Část 1 tohoto článku bude vypadat ve formátu x--www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="26b61-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="26b61-127">[Část 2](sending-html-form-data-part-2.md) popisuje vícedílné zprávy standardu MIME.</span><span class="sxs-lookup"><span data-stu-id="26b61-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="26b61-128">Odesílání komplexních typů</span><span class="sxs-lookup"><span data-stu-id="26b61-128">Sending Complex Types</span></span>

<span data-ttu-id="26b61-129">Obvykle se odesílají komplexní typ, skládající se z hodnot z několika ovládacích prvků formuláře.</span><span class="sxs-lookup"><span data-stu-id="26b61-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="26b61-130">Vezměte v úvahu následující model, který představuje stav aktualizace:</span><span class="sxs-lookup"><span data-stu-id="26b61-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="26b61-131">Tady je kontroler Web API, která přijímá `Update` objekt přes POST.</span><span class="sxs-lookup"><span data-stu-id="26b61-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="26b61-132">Tento kontroler používá [směrování na základě akce](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), takže je šablona trasy &quot;rozhraní api / {controller} / {action} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="26b61-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="26b61-133">Klient bude publikovat data, která mají &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="26b61-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="26b61-134">Nyní napíšeme formuláře HTML pro uživatele k odeslání aktualizace stavu.</span><span class="sxs-lookup"><span data-stu-id="26b61-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="26b61-135">Všimněte si, **akce** atributu ve formuláři je identifikátor URI naší akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="26b61-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="26b61-136">Tady je formulář s některé hodnoty zadané v:</span><span class="sxs-lookup"><span data-stu-id="26b61-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="26b61-137">Když uživatel klepne na tlačítko Odeslat, prohlížeč odešle požadavek HTTP podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="26b61-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="26b61-138">Všimněte si, že text požadavku obsahuje data formuláře, formátovaný jako dvojice název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="26b61-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="26b61-139">Webové rozhraní API automaticky převede dvojice název/hodnota do instance `Update` třídy.</span><span class="sxs-lookup"><span data-stu-id="26b61-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="26b61-140">Posílání dat formulářů prostřednictvím AJAX</span><span class="sxs-lookup"><span data-stu-id="26b61-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="26b61-141">Když uživatel odešle formulář, prohlížeči přejde mimo aktuální stránku a vykreslí tělo zprávy s odpovědí.</span><span class="sxs-lookup"><span data-stu-id="26b61-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="26b61-142">Který je v pořádku. Pokud je odpověď na stránku HTML.</span><span class="sxs-lookup"><span data-stu-id="26b61-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="26b61-143">S webovým rozhraním API, ale text odpovědi je obvykle buď prázdná, nebo obsahuje strukturovaná data, jako je například JSON.</span><span class="sxs-lookup"><span data-stu-id="26b61-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="26b61-144">V takovém případě je vhodnější k odesílání dat formuláře pomocí AJAX vyžádání, tak, aby stránky mohou zpracovat odpověď.</span><span class="sxs-lookup"><span data-stu-id="26b61-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="26b61-145">Následující kód ukazuje, jak publikovat data formuláře pomocí jQuery.</span><span class="sxs-lookup"><span data-stu-id="26b61-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="26b61-146">JQuery **odeslat** funkce nahradí akce formuláře s novou funkci.</span><span class="sxs-lookup"><span data-stu-id="26b61-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="26b61-147">Tím se přepíše výchozí chování tlačítka pro odeslání.</span><span class="sxs-lookup"><span data-stu-id="26b61-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="26b61-148">**Serializovat** funkce serializuje data formuláře na páry název/hodnota.</span><span class="sxs-lookup"><span data-stu-id="26b61-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="26b61-149">Chcete-li data formuláře odeslat k serveru, zavolejte `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="26b61-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="26b61-150">Po dokončení žádosti `.success()` nebo `.error()` obslužné rutiny zobrazí odpovídající zprávu pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="26b61-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="26b61-151">Odesílání jednoduché typy</span><span class="sxs-lookup"><span data-stu-id="26b61-151">Sending Simple Types</span></span>

<span data-ttu-id="26b61-152">V předchozích částech jsme vám poslali komplexní typ, který webového rozhraní API se deserializovala na instanci třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="26b61-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="26b61-153">Můžete také odeslat jednoduché typy, jako je například řetězec.</span><span class="sxs-lookup"><span data-stu-id="26b61-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="26b61-154">Před odesláním jednoduchého typu, zvažte možnost uzavřít hodnotu v komplexního typu. místo toho.</span><span class="sxs-lookup"><span data-stu-id="26b61-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="26b61-155">To poskytuje výhody model ověření na straně serveru a usnadňuje rozšíření modelu, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="26b61-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="26b61-156">Toto jsou základní kroky k odeslání jednoduchý typ stejný, ale existují dva drobné rozdíly.</span><span class="sxs-lookup"><span data-stu-id="26b61-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="26b61-157">V kontroleru, musíte nejprve uspořádání název parametru se **FromBody** atribut.</span><span class="sxs-lookup"><span data-stu-id="26b61-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="26b61-158">Ve výchozím nastavení se webové rozhraní API pokusí získat jednoduché typy z identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="26b61-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="26b61-159">**FromBody** atribut oznamuje webového rozhraní API načíst hodnotu z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="26b61-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="26b61-160">Webové rozhraní API přečte text odpovědi nejvýše jednou, takže pouze jeden parametr akce můžou pocházet z textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="26b61-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="26b61-161">Pokud je potřeba získat několik hodnot z textu požadavku, definice komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="26b61-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="26b61-162">Za druhé klient musí k odeslání hodnoty v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="26b61-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="26b61-163">Konkrétně část název z dvojice název/hodnota musí být prázdná pro jednoduchý typ.</span><span class="sxs-lookup"><span data-stu-id="26b61-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="26b61-164">Některé prohlížeče nepodporují to pro formuláře HTML, ale následujícím způsobem vytvořte tento formát ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="26b61-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="26b61-165">Tady je ukázkový formulář:</span><span class="sxs-lookup"><span data-stu-id="26b61-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="26b61-166">A tady je skript, který chcete odeslat hodnota formuláře.</span><span class="sxs-lookup"><span data-stu-id="26b61-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="26b61-167">Jediný rozdíl oproti předchozí skript je argument předaný do **příspěvku** funkce.</span><span class="sxs-lookup"><span data-stu-id="26b61-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="26b61-168">Stejný přístup můžete použít k odesílání pole jednoduchých typů:</span><span class="sxs-lookup"><span data-stu-id="26b61-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="26b61-169">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="26b61-169">Additional Resources</span></span>

[<span data-ttu-id="26b61-170">Část 2: Nahrání souboru a vícedílné zprávy standardu MIME</span><span class="sxs-lookup"><span data-stu-id="26b61-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
