---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Ověření v rozhraní ASP.NET Web API modelu | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393093"
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="0c93a-102">Ověření modelu v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="0c93a-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="0c93a-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0c93a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0c93a-104">Když klient odešle data do webového rozhraní API, často budete chtít ověřit data před provedením jakékoli zpracování.</span><span class="sxs-lookup"><span data-stu-id="0c93a-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="0c93a-105">Tento článek popisuje postup přidání poznámek ke své modely, použijte poznámky ověřování dat a zpracování chyb ověření webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0c93a-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0c93a-106">Datové poznámky</span><span class="sxs-lookup"><span data-stu-id="0c93a-106">Data Annotations</span></span>

<span data-ttu-id="0c93a-107">V rozhraní ASP.NET Web API, můžete použít atributy [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) oboru názvů k nastavení ověřovacích pravidel pro vlastnosti v modelu.</span><span class="sxs-lookup"><span data-stu-id="0c93a-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="0c93a-108">Vezměte v úvahu následující model:</span><span class="sxs-lookup"><span data-stu-id="0c93a-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="0c93a-109">Pokud používáte ověření modelu v architektuře ASP.NET MVC, to by měla vypadat povědomě.</span><span class="sxs-lookup"><span data-stu-id="0c93a-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="0c93a-110">**Vyžaduje** atribut uvádí, že `Name` vlastnost nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="0c93a-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="0c93a-111">**Rozsah** atribut uvádí, že `Weight` musí být v rozmezí od 0 do 999.</span><span class="sxs-lookup"><span data-stu-id="0c93a-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="0c93a-112">Předpokládejme, že klient odešle požadavek POST s následující reprezentaci JSON:</span><span class="sxs-lookup"><span data-stu-id="0c93a-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="0c93a-113">Uvidíte, že klient nenabízela `Name` vlastnost, která je označena jako povinné.</span><span class="sxs-lookup"><span data-stu-id="0c93a-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="0c93a-114">Pokud webové rozhraní API převede text JSON do `Product` instance, ověří `Product` proti atributy ověření.</span><span class="sxs-lookup"><span data-stu-id="0c93a-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="0c93a-115">Do vaší akce kontroleru můžete zkontrolovat, zda je model platný:</span><span class="sxs-lookup"><span data-stu-id="0c93a-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="0c93a-116">Ověření modelu nezaručuje, že klient data budou v bezpečí.</span><span class="sxs-lookup"><span data-stu-id="0c93a-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="0c93a-117">V jiných vrstvách aplikace může být nutné další ověřování.</span><span class="sxs-lookup"><span data-stu-id="0c93a-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="0c93a-118">(Například datová vrstva může vynutit omezení cizího klíče.) Tento kurz [pomocí webového rozhraní API s Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) zkoumá některé z těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="0c93a-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="0c93a-119">**"Nadměrném účtování"**: snížení účtování se stane, když klient vynechány některé vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0c93a-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="0c93a-120">Předpokládejme například, že klient odešle následující:</span><span class="sxs-lookup"><span data-stu-id="0c93a-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="0c93a-121">Tady, klient nezadali hodnoty `Price` nebo `Weight`.</span><span class="sxs-lookup"><span data-stu-id="0c93a-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="0c93a-122">Formátování JSON přiřadí výchozí hodnota nula, která chybí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0c93a-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="0c93a-123">Stav modelu, který je platný, protože nula je platná hodnota pro tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0c93a-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="0c93a-124">Ať už se jedná o problém závisí na vaší situaci.</span><span class="sxs-lookup"><span data-stu-id="0c93a-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="0c93a-125">Například v operaci aktualizace, můžete chtít rozlišovat mezi "0" a "není nastavený."</span><span class="sxs-lookup"><span data-stu-id="0c93a-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="0c93a-126">Vynuťte u klientů nastavit hodnotu, zkontrolujte vlastnost s možnou hodnotou Null a nastavené **vyžaduje** atribut:</span><span class="sxs-lookup"><span data-stu-id="0c93a-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="0c93a-127">**"Over-pass-the účtování"**: klienta můžete také odeslat *Další* dat, než jste očekávali.</span><span class="sxs-lookup"><span data-stu-id="0c93a-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="0c93a-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0c93a-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="0c93a-129">Tady, JSON obsahuje vlastnost ("Color"), která neexistuje v `Product` modelu.</span><span class="sxs-lookup"><span data-stu-id="0c93a-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="0c93a-130">V takovém případě formátování JSON jednoduše ignoruje tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="0c93a-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="0c93a-131">(Formátovací modul XML dělá to samé.) Útoky over-pass-the účtování způsobuje problémy, pokud model obsahuje vlastnosti, které jste chtěli být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="0c93a-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="0c93a-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="0c93a-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="0c93a-133">Nechcete, aby uživatelům aktualizovat `IsAdmin` vlastnost a sami zvýšit oprávnění na správce.</span><span class="sxs-lookup"><span data-stu-id="0c93a-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="0c93a-134">Nejbezpečnější možností je používat třídu modelu, který přesně odpovídá co klientovi může odeslat:</span><span class="sxs-lookup"><span data-stu-id="0c93a-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="0c93a-135">Příspěvek na blogu Brada Wilsona "[ověření vstupu vs. Ověření v architektuře ASP.NET MVC modelu](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"je dobré diskuzi o navýšení účtování a snížení účtování.</span><span class="sxs-lookup"><span data-stu-id="0c93a-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="0c93a-136">I když je příspěvek o ASP.NET MVC 2, problémy jsou stále relevantní pro webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="0c93a-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="0c93a-137">Zpracování chyb při ověřování</span><span class="sxs-lookup"><span data-stu-id="0c93a-137">Handling Validation Errors</span></span>

<span data-ttu-id="0c93a-138">Webové rozhraní API nevrací automaticky chybu klientovi když se ověřování nezdaří.</span><span class="sxs-lookup"><span data-stu-id="0c93a-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="0c93a-139">Záleží akce kontroleru ke kontrole stavu modelu a reagují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="0c93a-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="0c93a-140">Můžete také vytvořit filtr akce ke kontrole stavu modelu před vyvolání akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0c93a-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="0c93a-141">Následující kód ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="0c93a-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="0c93a-142">Pokud selže ověření modelu, tento filtr vrátí odpověď HTTP, který obsahuje chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="0c93a-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="0c93a-143">V takovém případě není vyvolána akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0c93a-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="0c93a-144">Chcete-li použít tento filtr na všechny řadiče webové rozhraní API, přidejte instance filtru, který **HttpConfiguration.Filters** kolekce během konfigurace:</span><span class="sxs-lookup"><span data-stu-id="0c93a-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="0c93a-145">Další možností je nastavit filtr na jednotlivých řadičích nebo akce kontroleru jako atribut:</span><span class="sxs-lookup"><span data-stu-id="0c93a-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
