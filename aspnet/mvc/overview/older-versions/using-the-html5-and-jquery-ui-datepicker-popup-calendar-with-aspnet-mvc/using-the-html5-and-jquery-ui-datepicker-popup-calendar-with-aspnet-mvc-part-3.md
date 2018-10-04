---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 3 | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery uživatelského rozhraní prvkem datepicker v MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: baaca74d584a6d5028824e877c561494cdff044e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577220"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="f0ede-103">Použití jazyka HTML5 a kalendáře jQuery UI Datepicker s architekturou ASP.NET MVC – část 3</span><span class="sxs-lookup"><span data-stu-id="f0ede-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="f0ede-104">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="f0ede-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="f0ede-105">V tomto kurzu se seznámíte se základy práce pomocí editoru šablon, šablony zobrazení a kalendářem jQuery UI datepicker v aplikaci MVC rozhraní ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="f0ede-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="f0ede-106">Práce s komplexní typy</span><span class="sxs-lookup"><span data-stu-id="f0ede-106">Working with Complex Types</span></span>

<span data-ttu-id="f0ede-107">V této části vytvoříte třídu adresu a zjistěte, jak vytvořit šablonu, která ho zobrazit.</span><span class="sxs-lookup"><span data-stu-id="f0ede-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="f0ede-108">V *modely* složku, vytvořte nový soubor třídy *Person.cs* místo, kam budete dáte dva typy: `Person` třídy a `Address` třídy.</span><span class="sxs-lookup"><span data-stu-id="f0ede-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="f0ede-109">`Person` Třídy bude obsahovat vlastnosti, která je zadána jako `Address`.</span><span class="sxs-lookup"><span data-stu-id="f0ede-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="f0ede-110">`Address` Typ je komplexní typ, což znamená není jeden z předdefinovaných typů, jako je `int`, `string`, nebo `double`.</span><span class="sxs-lookup"><span data-stu-id="f0ede-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="f0ede-111">Místo toho má několik vlastností.</span><span class="sxs-lookup"><span data-stu-id="f0ede-111">Instead, it has several properties.</span></span> <span data-ttu-id="f0ede-112">Kód pro nové třídy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f0ede-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="f0ede-113">V `Movie` kontroleru, přidejte následující `PersonDetail` akce k zobrazení instance osoby:</span><span class="sxs-lookup"><span data-stu-id="f0ede-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="f0ede-114">Pak přidejte následující kód, který `Movie` kontroleru k naplnění `Person` modelů s ukázkovými daty:</span><span class="sxs-lookup"><span data-stu-id="f0ede-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="f0ede-115">Otevřít *Views\Movies\PersonDetail.cshtml* a přidejte následující kód pro `PersonDetail` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f0ede-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="f0ede-116">Stisknutím kláves Ctrl + F5 spusťte aplikaci a přejděte do *filmy/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="f0ede-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="f0ede-117">`PersonDetail` Zobrazení neobsahuje `Address` komplexní typ, jako je vidět na tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="f0ede-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="f0ede-118">(Žádná adresa se zobrazí.)</span><span class="sxs-lookup"><span data-stu-id="f0ede-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="f0ede-119">`Address` Datového modelu se nezobrazí, protože se jedná o komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="f0ede-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="f0ede-120">Chcete-li zobrazit informace o adrese, otevřete *Views\Movies\PersonDetail.cshtml* znovu a přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="f0ede-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="f0ede-121">Kompletní kód pro `PersonDetail` teď zobrazení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f0ede-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="f0ede-122">Spusťte aplikaci znovu spustit a zobrazit `PersonDetail` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f0ede-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="f0ede-123">Informace o adrese se nyní zobrazí:</span><span class="sxs-lookup"><span data-stu-id="f0ede-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="f0ede-124">Vytvoření šablony pro komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="f0ede-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="f0ede-125">V této části vytvoříte šablonu, která se použije k vykreslení `Address` komplexního typu.</span><span class="sxs-lookup"><span data-stu-id="f0ede-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="f0ede-126">Když vytvoříte šablonu pro `Address` typ, ASP.NET MVC automaticky slouží k formátování modelu adresu kdekoli v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0ede-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="f0ede-127">To vám umožňuje řídit vykreslování `Address` typ z jediného místa v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0ede-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="f0ede-128">V *views\shared\displaytemplates za účelem nalezení* složku vytvořit částečné zobrazení silného typu, s názvem **adresu**:</span><span class="sxs-lookup"><span data-stu-id="f0ede-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="f0ede-129">Klikněte na tlačítko **přidat**a pak otevřete nový *Views\Shared\DisplayTemplates\Address.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="f0ede-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="f0ede-130">Nové zobrazení obsahuje následující vygenerované značky:</span><span class="sxs-lookup"><span data-stu-id="f0ede-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="f0ede-131">Spusťte aplikaci a zobrazit `PersonDetail` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f0ede-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="f0ede-132">Tentokrát `Address` šablonu, kterou jste právě vytvořili, se používá k zobrazení `Address` komplexní typ, tak zobrazení vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f0ede-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="f0ede-133">Shrnutí: Způsoby, jak určit formát zobrazení modelu a šablony</span><span class="sxs-lookup"><span data-stu-id="f0ede-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="f0ede-134">Už víte, že můžete určit formát nebo šablonu pro vlastnosti modelu pomocí následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="f0ede-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="f0ede-135">Použití `DisplayFormat` atribut na vlastnost v modelu.</span><span class="sxs-lookup"><span data-stu-id="f0ede-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="f0ede-136">Například následující kód způsobí, že data budou zobrazeny bez času:</span><span class="sxs-lookup"><span data-stu-id="f0ede-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="f0ede-137">Použití [datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atribut na vlastnost v modelu a určení datového typu.</span><span class="sxs-lookup"><span data-stu-id="f0ede-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="f0ede-138">Například následující kód způsobí, že data budou zobrazeny bez času.</span><span class="sxs-lookup"><span data-stu-id="f0ede-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="f0ede-139">Pokud aplikace obsahuje soubor *date.cshtml* šablony v *views\shared\displaytemplates za účelem nalezení* složky nebo *Views\Movies\DisplayTemplates* složky, tato šablona se použije k vykreslení `DateTime` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f0ede-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="f0ede-140">Integrované systémové šablon ASP.NET jinak zobrazí vlastnost jako datum.</span><span class="sxs-lookup"><span data-stu-id="f0ede-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="f0ede-141">Vytvoření šablony v zobrazení *views\shared\displaytemplates za účelem nalezení* složky nebo *Views\Movies\DisplayTemplates* složku, jejíž název odpovídá datový typ, který má být zformátován.</span><span class="sxs-lookup"><span data-stu-id="f0ede-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="f0ede-142">Například jste si všimli, že *Views\Shared\DisplayTemplates\DateTime.cshtml* byla použita k vykreslení `DateTime` vlastnosti v modelu, bez přidání atributu do modelu a bez nutnosti přidávat žádné značky k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f0ede-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="f0ede-143">Použití [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atribut v modelu, který má zadat šablonu, kterou chcete zobrazit vlastnosti modelu.</span><span class="sxs-lookup"><span data-stu-id="f0ede-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="f0ede-144">Explicitním přidáním zobrazovaný název šablony, čímž [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) volání v zobrazení.</span><span class="sxs-lookup"><span data-stu-id="f0ede-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="f0ede-145">Přístup, který použijete, závisí na co je potřeba udělat v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f0ede-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="f0ede-146">Není například neobvyklé kombinovat těchto přístupů, jaká formátování, které potřebujete získat.</span><span class="sxs-lookup"><span data-stu-id="f0ede-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="f0ede-147">V další části, bude přepnout trochu zařízením a přesouvat přizpůsobení zobrazení dat k přizpůsobení, jak se zadají.</span><span class="sxs-lookup"><span data-stu-id="f0ede-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="f0ede-148">Budete připojení datepicker jQuery na úpravy zobrazení v aplikaci, abyste uhlazený způsob, jak zadat data.</span><span class="sxs-lookup"><span data-stu-id="f0ede-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f0ede-149">[Předchozí](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [další](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="f0ede-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
