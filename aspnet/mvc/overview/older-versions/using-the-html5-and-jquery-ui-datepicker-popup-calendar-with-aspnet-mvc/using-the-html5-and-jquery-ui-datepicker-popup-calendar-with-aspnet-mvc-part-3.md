---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 3 | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869462"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="6b857-103">Použití jazyka HTML5 a kalendáře jQuery UI DatePicker s architekturou ASP.NET MVC – část 3</span><span class="sxs-lookup"><span data-stu-id="6b857-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="6b857-104">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6b857-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6b857-105">V tomto kurzu naučit základní informace o tom, jak pracovat s editor šablon, zobrazení šablon a kalendářem jQuery UI ovládací prvek datepicker v aplikaci ASP.NET MVC Web.</span><span class="sxs-lookup"><span data-stu-id="6b857-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="6b857-106">Práce s komplexní typy</span><span class="sxs-lookup"><span data-stu-id="6b857-106">Working with Complex Types</span></span>

<span data-ttu-id="6b857-107">V této části vytvoříte třídu adresu a zjistěte, jak vytvořit šablonu, můžete ho zobrazit.</span><span class="sxs-lookup"><span data-stu-id="6b857-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="6b857-108">V *modely* složky, vytvořte nový soubor třídy s názvem *Person.cs* kam budete umísťovat dva typy: `Person` – třída a `Address` třídy.</span><span class="sxs-lookup"><span data-stu-id="6b857-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="6b857-109">`Person` Třída bude obsahovat vlastnosti, která je zadán jako `Address`.</span><span class="sxs-lookup"><span data-stu-id="6b857-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="6b857-110">`Address` Typ je komplexního typu, což znamená, není jeden z předdefinovaných typů jako `int`, `string`, nebo `double`.</span><span class="sxs-lookup"><span data-stu-id="6b857-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="6b857-111">Místo toho má několik vlastností.</span><span class="sxs-lookup"><span data-stu-id="6b857-111">Instead, it has several properties.</span></span> <span data-ttu-id="6b857-112">Kód pro nové třídy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6b857-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="6b857-113">V `Movie` řadiče, přidejte následující `PersonDetail` akce k zobrazení instance osoby:</span><span class="sxs-lookup"><span data-stu-id="6b857-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="6b857-114">Pak přidejte následující kód, který `Movie` řadiče k naplnění `Person` modelu s ukázková data:</span><span class="sxs-lookup"><span data-stu-id="6b857-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="6b857-115">Otevřete *Views\Movies\PersonDetail.cshtml* souboru a přidejte následující kód pro `PersonDetail` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6b857-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="6b857-116">Stiskněte klávesu Ctrl + F5 a spusťte aplikaci a přejděte do *filmy nebo PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="6b857-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="6b857-117">`PersonDetail` Zobrazení neobsahuje `Address` komplexního typu, jako je můžete zobrazit na tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="6b857-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="6b857-118">(Žádná adresa se zobrazí.)</span><span class="sxs-lookup"><span data-stu-id="6b857-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="6b857-119">`Address` Data modelu není zobrazena, protože je komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="6b857-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="6b857-120">Chcete-li zobrazit informace o adrese, otevřete *Views\Movies\PersonDetail.cshtml* znovu a přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="6b857-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="6b857-121">Kód dokončení pro `PersonDetail` teď zobrazení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6b857-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="6b857-122">Spusťte aplikaci znovu a zobrazit `PersonDetail` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6b857-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="6b857-123">Informace o adrese se nyní zobrazí:</span><span class="sxs-lookup"><span data-stu-id="6b857-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="6b857-124">Vytvoření šablony pro komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="6b857-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="6b857-125">V této části vytvoříte šablonu, která se použije k vykreslení `Address` komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="6b857-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="6b857-126">Když vytvoříte šablonu pro `Address` typu, ASP.NET MVC mohou automaticky používat ji k formátování model adresu kdekoli v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b857-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="6b857-127">To vám umožňuje řídit vykreslování `Address` typu z jenom jednom místě v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b857-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="6b857-128">V *Views\Shared\DisplayTemplates* složky, vytvoření částečné zobrazení silného typu, s názvem **adresu**:</span><span class="sxs-lookup"><span data-stu-id="6b857-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="6b857-129">Klikněte na tlačítko **přidat**a pak otevřete nový *Views\Shared\DisplayTemplates\Address.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="6b857-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="6b857-130">Nové zobrazení obsahuje následující generovaný kód:</span><span class="sxs-lookup"><span data-stu-id="6b857-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="6b857-131">Spusťte aplikaci a zobrazit `PersonDetail` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6b857-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="6b857-132">Tentokrát `Address` šablonu, kterou jste právě vytvořili, se používá k zobrazení `Address` komplexní typ, takže zobrazení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="6b857-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="6b857-133">Souhrn: Způsobů určení modelu formát zobrazení a šablony</span><span class="sxs-lookup"><span data-stu-id="6b857-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="6b857-134">Už víte, že je možné zadat formát nebo šablonu pro vlastnosti modelu pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="6b857-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="6b857-135">Použití `DisplayFormat` atribut na vlastnost v modelu.</span><span class="sxs-lookup"><span data-stu-id="6b857-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="6b857-136">Například následující kód způsobí, že data budou zobrazeny bez čas:</span><span class="sxs-lookup"><span data-stu-id="6b857-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="6b857-137">Použití [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) na vlastnost v modelu a zadání datový typ atributu.</span><span class="sxs-lookup"><span data-stu-id="6b857-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="6b857-138">Například následující kód způsobí, že data budou zobrazeny bez čas.</span><span class="sxs-lookup"><span data-stu-id="6b857-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="6b857-139">Pokud aplikace obsahuje *date.cshtml* šablonu *Views\Shared\DisplayTemplates* složky nebo *Views\Movies\DisplayTemplates* složku, že šablona slouží k vykreslení `DateTime` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6b857-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="6b857-140">V opačném případě předdefinovaného systémového ukázka ASP.NET zobrazí vlastnost jako datum.</span><span class="sxs-lookup"><span data-stu-id="6b857-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="6b857-141">Vytvoření šablony v zobrazení *Views\Shared\DisplayTemplates* složku nebo *Views\Movies\DisplayTemplates* složku, jejíž název odpovídá typu dat, který chcete formátovat.</span><span class="sxs-lookup"><span data-stu-id="6b857-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="6b857-142">Například, který jste viděli *Views\Shared\DisplayTemplates\DateTime.cshtml* byla použita k vykreslení `DateTime` vlastnosti v modelu, bez přidání atributu do modelu a bez přidání jakékoli značek k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6b857-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="6b857-143">Pomocí [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributů v modelu, který má zadejte šablonu, kterou chcete zobrazit vlastnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="6b857-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="6b857-144">Přidání explicitně zobrazovaný název šablony na [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) volání v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6b857-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="6b857-145">Přístupů, které použijete, závisí na co musíte udělat v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6b857-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="6b857-146">Není kombinovat tyto přístupy k získání přesně druh formátování, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6b857-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="6b857-147">V další části přepínat trochu zařízením a přesunout z přizpůsobení zobrazení dat k přizpůsobení, jak je zadána.</span><span class="sxs-lookup"><span data-stu-id="6b857-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="6b857-148">Budete spojit ovládací prvek datepicker jQuery k zobrazení upravit v aplikaci, aby bylo možné poskytnout slick způsob, jak zadat data.</span><span class="sxs-lookup"><span data-stu-id="6b857-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6b857-149">[Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="6b857-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
