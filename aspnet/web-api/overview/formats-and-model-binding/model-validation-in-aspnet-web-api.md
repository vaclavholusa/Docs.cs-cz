---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model ověřování v rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="b26c5-102">Ověření modelu v rozhraní ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b26c5-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b26c5-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b26c5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b26c5-104">Když klient odešle data do webového rozhraní API, často budete chtít ověřit data před provedením jakékoli zpracovávání.</span><span class="sxs-lookup"><span data-stu-id="b26c5-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="b26c5-105">Tento článek ukazuje, jak opatřit poznámkami modely, použijte poznámky pro ověření dat a zpracování chyb při ověřování v webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b26c5-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b26c5-106">Datových poznámek</span><span class="sxs-lookup"><span data-stu-id="b26c5-106">Data Annotations</span></span>

<span data-ttu-id="b26c5-107">V rozhraní ASP.NET Web API, můžete použít atributy z [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) obor názvů nastavit pravidla ověřování pro vlastnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="b26c5-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="b26c5-108">Vezměte v úvahu následující modelu:</span><span class="sxs-lookup"><span data-stu-id="b26c5-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="b26c5-109">Pokud jste použili ověření modelu v architektuře ASP.NET MVC, to by měla vypadat povědomě.</span><span class="sxs-lookup"><span data-stu-id="b26c5-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="b26c5-110">**Požadované** atribut uvádí, že `Name` vlastnost nesmí mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="b26c5-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="b26c5-111">**Rozsah** atribut uvádí, že `Weight` musí být v rozmezí od 0 do 999.</span><span class="sxs-lookup"><span data-stu-id="b26c5-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="b26c5-112">Předpokládejme, že klient odešle požadavek POST s následující reprezentace JSON:</span><span class="sxs-lookup"><span data-stu-id="b26c5-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="b26c5-113">Uvidíte, že klient nezahrnuli `Name` požadované vlastnosti, která je označena jako.</span><span class="sxs-lookup"><span data-stu-id="b26c5-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="b26c5-114">Když webového rozhraní API převede do formátu JSON `Product` instance, ověřuje `Product` proti atributy ověření.</span><span class="sxs-lookup"><span data-stu-id="b26c5-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="b26c5-115">V řadiči akci můžete zkontrolovat, zda je model platný:</span><span class="sxs-lookup"><span data-stu-id="b26c5-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="b26c5-116">Ověření modelu není zaručeno, že data klientů je bezpečné.</span><span class="sxs-lookup"><span data-stu-id="b26c5-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="b26c5-117">Další ověření může být vhodné v jiných vrstev aplikace.</span><span class="sxs-lookup"><span data-stu-id="b26c5-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="b26c5-118">(Například datová vrstva může vynutit omezení cizích klíčů.) Tento kurz [pomocí webového rozhraní API s platformou Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) jsou zde popsány některé z těchto problémů.</span><span class="sxs-lookup"><span data-stu-id="b26c5-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="b26c5-119">**"Snížení příspěvků"**: snížení účtování se stane, když klient zůstane se některé vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b26c5-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="b26c5-120">Předpokládejme například, že klient odešle následující:</span><span class="sxs-lookup"><span data-stu-id="b26c5-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="b26c5-121">Zde klienta nebyla zadána hodnota pro `Price` nebo `Weight`.</span><span class="sxs-lookup"><span data-stu-id="b26c5-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="b26c5-122">Formátovací modul JSON přiřadí výchozí hodnotu nula na chybějící vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b26c5-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="b26c5-123">Stav modelu, který je platný, protože nula není platná hodnota pro tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b26c5-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="b26c5-124">Zda se jedná o problém závisí na vašem scénáři.</span><span class="sxs-lookup"><span data-stu-id="b26c5-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="b26c5-125">Například v operaci aktualizace, můžete k rozlišení mezi "nula" a "není nastavena."</span><span class="sxs-lookup"><span data-stu-id="b26c5-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="b26c5-126">Chcete-li vynutit klientům nastavit hodnotu, zkontrolujte vlastnost s možnou hodnotou Null a nastavte **požadované** atribut:</span><span class="sxs-lookup"><span data-stu-id="b26c5-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="b26c5-127">**"Typu Overpost"**: klient může také odesílat *Další* dat, než jste očekávali.</span><span class="sxs-lookup"><span data-stu-id="b26c5-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="b26c5-128">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b26c5-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="b26c5-129">Zde JSON obsahuje vlastnosti ("Barva"), která neexistuje v `Product` modelu.</span><span class="sxs-lookup"><span data-stu-id="b26c5-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="b26c5-130">V takovém případě formátovací modul JSON jednoduše ignoruje tato hodnota.</span><span class="sxs-lookup"><span data-stu-id="b26c5-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="b26c5-131">(Formátovací modul XML nemá stejný.) Přečerpání příspěvků způsobuje problémy, pokud model obsahuje vlastnosti, které mají být jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="b26c5-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="b26c5-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b26c5-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="b26c5-133">Nechcete, aby uživatelům aktualizovat `IsAdmin` vlastnost a sami zvýšení oprávnění správcům!</span><span class="sxs-lookup"><span data-stu-id="b26c5-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="b26c5-134">Nejbezpečnější strategie se má používat třídu modelu, který přesně odpovídá má klient povoleno odeslat:</span><span class="sxs-lookup"><span data-stu-id="b26c5-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="b26c5-135">Příspěvek blogu Brada Wilsona "[ověření vstupu vs. Model ověření v architektuře ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"má dobrou diskuzi o snížení účtování a přečerpání účtování.</span><span class="sxs-lookup"><span data-stu-id="b26c5-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="b26c5-136">I když je požadavek post o ASP.NET MVC 2, problémy jsou stále relevantní pro webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b26c5-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="b26c5-137">Zpracování chyb při ověřování</span><span class="sxs-lookup"><span data-stu-id="b26c5-137">Handling Validation Errors</span></span>

<span data-ttu-id="b26c5-138">Webové rozhraní API nevrací automaticky chybu klientovi když se ověřování nezdaří.</span><span class="sxs-lookup"><span data-stu-id="b26c5-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="b26c5-139">Je akce kontroleru ke kontrole stavu modelu a reagují odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="b26c5-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="b26c5-140">Můžete také vytvořit filtr akce pro kontrolu stavu modelu, před vyvoláním akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b26c5-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="b26c5-141">Následující kód ukazuje příklad:</span><span class="sxs-lookup"><span data-stu-id="b26c5-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="b26c5-142">Pokud selže ověření modelu, tento filtr vrátí odpověď HTTP, který obsahuje chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="b26c5-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="b26c5-143">V takovém případě není vyvolána akce kontroleru.</span><span class="sxs-lookup"><span data-stu-id="b26c5-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="b26c5-144">Tento filtr použít na všechny řadiče webového rozhraní API, přidá instanci filtr, který má **HttpConfiguration.Filters** kolekce během konfigurace:</span><span class="sxs-lookup"><span data-stu-id="b26c5-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="b26c5-145">Další možností je nastavit filtr na jednotlivých řadičích nebo akce kontroleru jako atribut:</span><span class="sxs-lookup"><span data-stu-id="b26c5-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
