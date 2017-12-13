---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: "Použití třídy TagBuilder k sestavení pomocné rutiny HTML (C#) | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther vás seznámí s třídy užitečné nástroj v rozhraní ASP.NET MVC s názvem třídy TagBuilder. Třídy pro TagBuilder můžete snadno..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc4e91bb14082c7c5e889d064d29d2bf91f7329
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a><span data-ttu-id="a864f-104">Použití třídy TagBuilder k sestavení pomocné rutiny HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="a864f-104">Using the TagBuilder Class to Build HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="a864f-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a864f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a864f-106">Stephen Walther vás seznámí s třídy užitečné nástroj v rozhraní ASP.NET MVC s názvem třídy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a864f-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="a864f-107">Třída TagBuilder můžete snadno vytvářet značky HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="a864f-108">Rozhraní ASP.NET MVC zahrnuje třídy užitečné nástroj s názvem TagBuilder třídu, která můžete použít při vytváření Pomocníci HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="a864f-109">Třída TagBuilder, jako název třídy naznačuje, umožňuje snadno vytvářet značky HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="a864f-110">V tomto kurzu stručný jsou k dispozici přehled TagBuilder třídy a další informace o použití této třídy, při vytváření jednoduchých pomocné rutiny HTML, který vykreslí HTML &lt;img&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="a864f-111">Přehled třídy TagBuilder</span><span class="sxs-lookup"><span data-stu-id="a864f-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="a864f-112">Třída TagBuilder je obsažena v oboru názvů System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="a864f-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="a864f-113">Má pět metody:</span><span class="sxs-lookup"><span data-stu-id="a864f-113">It has five methods:</span></span>

- <span data-ttu-id="a864f-114">AddCssClass() - umožňuje přidat nový *třídy = ""* atribut značku.</span><span class="sxs-lookup"><span data-stu-id="a864f-114">AddCssClass() - Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="a864f-115">GenerateId() - umožňuje přidat atribut id značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-115">GenerateId() - Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="a864f-116">Tato metoda automaticky nahrazuje tečky v id (ve výchozím nastavení, tečky jsou nahrazovány podtržítka)</span><span class="sxs-lookup"><span data-stu-id="a864f-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="a864f-117">MergeAttribute() - umožňuje přidat atributy pro značku.</span><span class="sxs-lookup"><span data-stu-id="a864f-117">MergeAttribute() - Enables you to add attributes to a tag.</span></span> <span data-ttu-id="a864f-118">Existuje více přetížení této metody.</span><span class="sxs-lookup"><span data-stu-id="a864f-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="a864f-119">SetInnerText() – umožňuje nastavit vnitřní text značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-119">SetInnerText() - Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="a864f-120">Vnitřní text je automaticky použije kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="a864f-121">ToString() - umožňuje vykreslovat značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-121">ToString() - Enables you to render the tag.</span></span> <span data-ttu-id="a864f-122">Můžete zadat, zda chcete vytvořit značku normální úvodní značky, koncová značka nebo samouzavírací značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="a864f-123">Třída TagBuilder má čtyři důležité vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="a864f-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="a864f-124">Atributy – představuje všechny atributy značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-124">Attributes - Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="a864f-125">IdAttributeDotReplacement - představuje znak metodou GenerateId() používá k nahrazení tečky (výchozí hodnota je podtržítko).</span><span class="sxs-lookup"><span data-stu-id="a864f-125">IdAttributeDotReplacement - Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="a864f-126">InnerHTML - představuje vnitřní obsah značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-126">InnerHTML - Represents the inner contents of the tag.</span></span> <span data-ttu-id="a864f-127">Přiřazení řetězec na tuto vlastnost *nemá* řetězec kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="a864f-128">TagName - představuje název značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-128">TagName - Represents the name of the tag.</span></span>

<span data-ttu-id="a864f-129">Tyto metody a vlastnosti, získáte všechny základní metody a vlastnosti, které potřebujete k vytváření až značky jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="a864f-130">Nepotřebujete skutečně použití třídy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a864f-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="a864f-131">Třídy StringBuilder můžete místo toho použít.</span><span class="sxs-lookup"><span data-stu-id="a864f-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="a864f-132">Však třídu TagBuilder usnadňuje život trochu.</span><span class="sxs-lookup"><span data-stu-id="a864f-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="a864f-133">Vytváření pomocné rutiny HTML bitové kopie</span><span class="sxs-lookup"><span data-stu-id="a864f-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="a864f-134">Při vytváření instance třídy TagBuilder předáte název značky, který chcete vytvořit TagBuilder konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="a864f-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="a864f-135">Dále můžete volat metody, třeba metody AddCssClass a MergeAttribute() upravit atributy značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="a864f-136">Nakonec zavolejte metodu ToString() k vykreslení značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="a864f-137">Například obsahuje výpis 1 pomocné rutiny HTML bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a864f-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="a864f-138">Pomocník bitové kopie je implementováno interně s TagBuilder, který představuje HTML &lt;img&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="a864f-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="a864f-139">**Výpis 1 - Helpers\ImageHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="a864f-139">**Listing 1 - Helpers\ImageHelper.cs**</span></span>

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

<span data-ttu-id="a864f-140">Třída v výpis 1 obsahuje dvě statické přetížené metody s názvem bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="a864f-140">The class in Listing 1 contains two static overloaded methods named Image.</span></span> <span data-ttu-id="a864f-141">Při volání metody Image(), můžete předat objekt, který představuje sadu atributů HTML, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="a864f-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="a864f-142">Všimněte si, jak TagBuilder.MergeAttribute() metoda se používá k přidání jednotlivé atributy, například atribut src k TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a864f-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="a864f-143">Všimněte si kromě toho, jak TagBuilder.MergeAttributes() metoda se používá k přidání kolekce atributů k TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="a864f-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="a864f-144">Slovník akceptuje metodu MergeAttributes()&lt;řetězec, objekt&gt; parametr.</span><span class="sxs-lookup"><span data-stu-id="a864f-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="a864f-145">Třída RouteValueDictionary slouží převést objekt představující kolekci atributů do slovníku&lt;řetězec, objekt&gt;.</span><span class="sxs-lookup"><span data-stu-id="a864f-145">The The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="a864f-146">Po vytvoření bitové kopie pomocné rutiny, můžete v zobrazení v rozhraní ASP.NET MVC stejně jako všechny ostatní standardní pomocníků HTML pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="a864f-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="a864f-147">Zobrazení v výpis 2 pomocí pomocníka Image dvakrát zobrazí stejné bitové kopie Xbox (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="a864f-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="a864f-148">Pomocník Image() je volána s i bez zadání kolekce atributů HTML.</span><span class="sxs-lookup"><span data-stu-id="a864f-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="a864f-149">**Výpis 2 - Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a864f-149">**Listing 2 - Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


<span data-ttu-id="a864f-150">[![Dialogové okno Nový projekt](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a864f-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="a864f-151">**Obrázek 01**: využitím pomocné rutiny bitové kopie ([Kliknutím zobrazit obrázek v plné velikosti](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a864f-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))</span></span>


<span data-ttu-id="a864f-152">Všimněte si, že je nutné naimportovat obor názvů související s pomocnou rutinou bitové kopie v horní části Index.aspx zobrazení.</span><span class="sxs-lookup"><span data-stu-id="a864f-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="a864f-153">Pomocné rutiny importu s direktivou následující:</span><span class="sxs-lookup"><span data-stu-id="a864f-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

>[!div class="step-by-step"]
<span data-ttu-id="a864f-154">[Předchozí](creating-custom-html-helpers-cs.md)
[další](creating-page-layouts-with-view-master-pages-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a864f-154">[Previous](creating-custom-html-helpers-cs.md)
[Next](creating-page-layouts-with-view-master-pages-cs.md)</span></span>
