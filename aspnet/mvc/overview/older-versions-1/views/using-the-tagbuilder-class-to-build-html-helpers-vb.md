---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: "Použití třídy TagBuilder k sestavení pomocné objekty HTML (VB) | Microsoft Docs"
author: StephenWalther
description: "Stephen Walther vás seznámí s třídy užitečné nástroj v rozhraní ASP.NET MVC s názvem třídy TagBuilder. Třídy pro TagBuilder můžete snadno..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 92c003cf929448d0b03f9de76330e9495ac51d20
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="81c5e-104">Použití třídy TagBuilder k sestavení pomocné objekty HTML (VB)</span><span class="sxs-lookup"><span data-stu-id="81c5e-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="81c5e-105">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="81c5e-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="81c5e-106">Stephen Walther vás seznámí s třídy užitečné nástroj v rozhraní ASP.NET MVC s názvem třídy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="81c5e-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="81c5e-107">Třída TagBuilder můžete snadno vytvářet značky HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="81c5e-108">Rozhraní ASP.NET MVC zahrnuje třídy užitečné nástroj s názvem TagBuilder třídu, která můžete použít při vytváření Pomocníci HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="81c5e-109">Třída TagBuilder, jako název třídy naznačuje, umožňuje snadno vytvářet značky HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="81c5e-110">V tomto kurzu stručný jsou k dispozici přehled TagBuilder třídy a další informace o použití této třídy, při vytváření jednoduchých pomocné rutiny HTML, který vykreslí HTML &lt;img&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="81c5e-111">Přehled třídy TagBuilder</span><span class="sxs-lookup"><span data-stu-id="81c5e-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="81c5e-112">Třída TagBuilder je obsažena v oboru názvů System.Web.Mvc.</span><span class="sxs-lookup"><span data-stu-id="81c5e-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="81c5e-113">Má pět metody:</span><span class="sxs-lookup"><span data-stu-id="81c5e-113">It has five methods:</span></span>

- <span data-ttu-id="81c5e-114">AddCssClass() – umožňuje přidat nový *třídy = ""* atribut značku.</span><span class="sxs-lookup"><span data-stu-id="81c5e-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="81c5e-115">GenerateId() – umožňuje přidat atribut id značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="81c5e-116">Tato metoda automaticky nahrazuje tečky v id (ve výchozím nastavení, tečky jsou nahrazovány podtržítka)</span><span class="sxs-lookup"><span data-stu-id="81c5e-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="81c5e-117">MergeAttribute() – umožňuje přidat atributy pro značku.</span><span class="sxs-lookup"><span data-stu-id="81c5e-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="81c5e-118">Existuje více přetížení této metody.</span><span class="sxs-lookup"><span data-stu-id="81c5e-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="81c5e-119">SetInnerText() – umožňuje nastavit vnitřní text značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="81c5e-120">Vnitřní text je automaticky použije kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="81c5e-121">ToString() – umožňuje vykreslovat značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="81c5e-122">Můžete zadat, zda chcete vytvořit značku normální úvodní značky, koncová značka nebo samouzavírací značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="81c5e-123">Třída TagBuilder má čtyři důležité vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="81c5e-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="81c5e-124">Atributy – reprezentuje všechny atributy značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="81c5e-125">IdAttributeDotReplacement – představuje znak metodou GenerateId() používá k nahrazení tečky (výchozí hodnota je podtržítko).</span><span class="sxs-lookup"><span data-stu-id="81c5e-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="81c5e-126">InnerHTML – představuje vnitřní obsah značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="81c5e-127">Přiřazení řetězec na tuto vlastnost *nemá* řetězec kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="81c5e-128">TagName – představuje název značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="81c5e-129">Tyto metody a vlastnosti, získáte všechny základní metody a vlastnosti, které potřebujete k vytváření až značky jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="81c5e-130">Nepotřebujete skutečně použití třídy TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="81c5e-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="81c5e-131">Třídy StringBuilder můžete místo toho použít.</span><span class="sxs-lookup"><span data-stu-id="81c5e-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="81c5e-132">Však třídu TagBuilder usnadňuje život trochu.</span><span class="sxs-lookup"><span data-stu-id="81c5e-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="81c5e-133">Vytváření pomocné rutiny HTML bitové kopie</span><span class="sxs-lookup"><span data-stu-id="81c5e-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="81c5e-134">Při vytváření instance třídy TagBuilder předáte název značky, který chcete vytvořit TagBuilder konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="81c5e-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="81c5e-135">Dále můžete volat metody, třeba metody AddCssClass a MergeAttribute() upravit atributy značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="81c5e-136">Nakonec zavolejte metodu ToString() k vykreslení značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="81c5e-137">Například obsahuje výpis 1 pomocné rutiny HTML bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="81c5e-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="81c5e-138">Pomocník bitové kopie je implementováno interně s TagBuilder, který představuje HTML &lt;img&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="81c5e-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="81c5e-139">**Výpis 1 – Helpers\ImageHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="81c5e-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="81c5e-140">Tento modul v výpis 1 obsahuje dvě přetížené metody s názvem Image().</span><span class="sxs-lookup"><span data-stu-id="81c5e-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="81c5e-141">Při volání metody Image(), můžete předat objekt, který představuje sadu atributů HTML, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="81c5e-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="81c5e-142">Všimněte si, jak TagBuilder.MergeAttribute() metoda se používá k přidání jednotlivé atributy, například atribut src k TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="81c5e-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="81c5e-143">Všimněte si kromě toho, jak TagBuilder.MergeAttributes() metoda se používá k přidání kolekce atributů k TagBuilder.</span><span class="sxs-lookup"><span data-stu-id="81c5e-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="81c5e-144">Slovník akceptuje metodu MergeAttributes()&lt;řetězec, objekt&gt; parametr.</span><span class="sxs-lookup"><span data-stu-id="81c5e-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="81c5e-145">Třída RouteValueDictionary slouží převést objekt představující kolekci atributů do slovníku&lt;řetězec, objekt&gt;.</span><span class="sxs-lookup"><span data-stu-id="81c5e-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="81c5e-146">Po vytvoření bitové kopie pomocné rutiny, můžete v zobrazení v rozhraní ASP.NET MVC stejně jako všechny ostatní standardní pomocníků HTML pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="81c5e-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="81c5e-147">Zobrazení v výpis 2 pomocí pomocníka Image dvakrát zobrazí stejné bitové kopie Xbox (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="81c5e-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="81c5e-148">Pomocník Image() je volána s i bez zadání kolekce atributů HTML.</span><span class="sxs-lookup"><span data-stu-id="81c5e-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="81c5e-149">**Výpis 2 – Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="81c5e-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


<span data-ttu-id="81c5e-150">[![Dialogové okno Nový projekt](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="81c5e-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="81c5e-151">**Obrázek 01**: využitím pomocné rutiny bitové kopie ([Kliknutím zobrazit obrázek v plné velikosti](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="81c5e-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>


<span data-ttu-id="81c5e-152">Všimněte si, že je nutné naimportovat obor názvů související s pomocnou rutinou bitové kopie v horní části Index.aspx zobrazení.</span><span class="sxs-lookup"><span data-stu-id="81c5e-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="81c5e-153">Pomocné rutiny importu s direktivou následující:</span><span class="sxs-lookup"><span data-stu-id="81c5e-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="81c5e-154">V aplikaci Visual Basic výchozí obor názvů je stejný jako název aplikace.</span><span class="sxs-lookup"><span data-stu-id="81c5e-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="81c5e-155">[Předchozí](creating-custom-html-helpers-vb.md)
[další](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="81c5e-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
