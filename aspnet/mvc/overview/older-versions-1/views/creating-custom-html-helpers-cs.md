---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Vytvoření vlastních pomocných rutin HTML (C#) | Dokumentace Microsoftu
author: microsoft
description: Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC. S využitím pomocné rutiny HTML...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839406"
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="c0420-104">Vytvoření vlastních pomocných rutin HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="c0420-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="c0420-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c0420-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="c0420-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="c0420-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="c0420-107">Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC.</span><span class="sxs-lookup"><span data-stu-id="c0420-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="c0420-108">S využitím pomocné rutiny HTML, můžete snížit množství tedious zadáním značky jazyka HTML, je nutné provést pro vytvoření standardní stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="c0420-109">Cílem tohoto kurzu je předvést, jak můžete vytvořit vlastní pomocné rutiny HTML, který vám pomůže v rámci zobrazení v rozhraní MVC.</span><span class="sxs-lookup"><span data-stu-id="c0420-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="c0420-110">S využitím pomocné rutiny HTML, můžete snížit množství tedious zadáním značky jazyka HTML, je nutné provést pro vytvoření standardní stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="c0420-111">V první části tohoto kurzu můžu popisují některé existující pomocných rutin HTML součástí rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c0420-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="c0420-112">V dalším kroku můžu popisují dva způsoby vytvoření vlastních pomocných rutin HTML: Mohu vysvětluje postup při vytvoření vlastních pomocných rutin HTML tak, že vytvoříte statické metody a tak, že vytvoříte metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c0420-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="c0420-113">Principy pomocných rutin HTML</span><span class="sxs-lookup"><span data-stu-id="c0420-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="c0420-114">Pomocné rutiny HTML je právě metodu, která vrátí hodnotu typu string.</span><span class="sxs-lookup"><span data-stu-id="c0420-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="c0420-115">Řetězec může představovat jakýkoli typ obsahu, které chcete.</span><span class="sxs-lookup"><span data-stu-id="c0420-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="c0420-116">Například můžete použít pomocných rutin HTML pro vykreslení standardní značky HTML, jako je HTML `<input>` a `<img>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="c0420-117">Pomocné rutiny HTML můžete použít také k vykreslení složitější obsah, jako jsou pruhu karet nebo HTML tabulku dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="c0420-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="c0420-118">Architektura ASP.NET MVC zahrnuje následující sadu standardních pomocných rutin HTML (Toto není kompletní seznam):</span><span class="sxs-lookup"><span data-stu-id="c0420-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="c0420-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="c0420-119">Html.ActionLink()</span></span>
- <span data-ttu-id="c0420-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="c0420-120">Html.BeginForm()</span></span>
- <span data-ttu-id="c0420-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="c0420-121">Html.CheckBox()</span></span>
- <span data-ttu-id="c0420-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="c0420-122">Html.DropDownList()</span></span>
- <span data-ttu-id="c0420-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="c0420-123">Html.EndForm()</span></span>
- <span data-ttu-id="c0420-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="c0420-124">Html.Hidden()</span></span>
- <span data-ttu-id="c0420-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="c0420-125">Html.ListBox()</span></span>
- <span data-ttu-id="c0420-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="c0420-126">Html.Password()</span></span>
- <span data-ttu-id="c0420-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="c0420-127">Html.RadioButton()</span></span>
- <span data-ttu-id="c0420-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="c0420-128">Html.TextArea()</span></span>
- <span data-ttu-id="c0420-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="c0420-129">Html.TextBox()</span></span>

<span data-ttu-id="c0420-130">Představte si třeba formulář v nástrojích pro výpis 1.</span><span class="sxs-lookup"><span data-stu-id="c0420-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="c0420-131">Tento formulář je vykreslen pomocí dvou standardní pomocných rutin HTML (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="c0420-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="c0420-132">Tento formulář používá `Html.BeginForm()` a `Html.TextBox()` pomocné metody pro vykreslení jednoduchý formulář HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="c0420-133">[![Vykreslí stránku s pomocných rutin HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c0420-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="c0420-134">**Obrázek 01**: stránka vybarvením pomocných rutin HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c0420-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="c0420-135">**Výpis 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c0420-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="c0420-136">Html.BeginForm() Pomocná metoda se používá k vytvoření HTML otevírací a zavírací `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="c0420-137">Všimněte si, že `Html.BeginForm()` metoda je volána v rámci pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="c0420-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="c0420-138">Příkaz zajistí, že `<form>` značky zavřena na konci pomocí bloku.</span><span class="sxs-lookup"><span data-stu-id="c0420-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="c0420-139">Pokud dáváte přednost, místo vytvořit pomocí bloku, můžete volat metodu Html.EndForm() Helper zavřete `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="c0420-140">Použít v obou případech vytváření otevření a zavření `<form>` značky, co nejvíce intuitivní pro vás.</span><span class="sxs-lookup"><span data-stu-id="c0420-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="c0420-141">`Html.TextBox()` Pomocné metody se používají v 1 zobrazení k vykreslení HTML `<input>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="c0420-142">Pokud zvolíte možnost Zobrazit zdroj ve vašem prohlížeči zobrazí zdroji HTML ve výpisu 2.</span><span class="sxs-lookup"><span data-stu-id="c0420-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="c0420-143">Všimněte si, že zdroj obsahuje standardní značky HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c0420-144">Všimněte si, že `Html.TextBox()`pomocné rutiny HTML – je vykreslen pomocí `<%= %>` značek namísto `<% %>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="c0420-145">Pokud nechcete zahrnout znaménka rovnosti, pak nic získá zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c0420-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="c0420-146">Architektura ASP.NET MVC obsahuje malou sadu pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="c0420-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="c0420-147">Pravděpodobně je potřeba rozšířit rozhraní MVC pomocí vlastních pomocných rutin HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="c0420-148">Ve zbývající části tohoto kurzu přečtěte si dvě metody vytvoření vlastních pomocných rutin HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="c0420-149">**Výpis 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="c0420-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="c0420-150">Vytváření pomocných rutin HTML pomocí statické metody</span><span class="sxs-lookup"><span data-stu-id="c0420-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="c0420-151">Nejjednodušší způsob, jak vytvořit nové pomocné rutiny HTML je vytvoření statická metoda, která vrátí hodnotu typu string.</span><span class="sxs-lookup"><span data-stu-id="c0420-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="c0420-152">Představte si třeba, abyste se rozhodli vytvořit nové pomocné rutiny HTML, který vykreslí HTML `<label>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="c0420-153">Třídy v informacích 2 můžete použít k vykreslení `<label>` .</span><span class="sxs-lookup"><span data-stu-id="c0420-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="c0420-154">**Výpis 2 – `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="c0420-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="c0420-155">Není nic zvláštního o třídě v informacích 2.</span><span class="sxs-lookup"><span data-stu-id="c0420-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="c0420-156">`Label()` Metoda jednoduše vrací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c0420-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="c0420-157">Používá upravený zobrazení indexu v informacích 3 `LabelHelper` k vykreslení HTML `<label>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="c0420-158">Všimněte si, že zobrazení zahrnuje `<%@ imports %>` – direktiva, která importuje `Application1.Helpers` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c0420-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="c0420-159">**Výpis 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c0420-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="c0420-160">Vytváření pomocných rutin HTML pomocí metod rozšíření</span><span class="sxs-lookup"><span data-stu-id="c0420-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="c0420-161">Pokud chcete vytvořit pomocné rutiny HTML, které fungují stejně jako standardní pomocných rutin HTML zahrnuty v rozhraní ASP.NET MVC, pak je potřeba vytvořit metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="c0420-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="c0420-162">Rozšiřující metody umožňují přidat nové metody do existující třídy.</span><span class="sxs-lookup"><span data-stu-id="c0420-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="c0420-163">Při vytváření metodu pomocné rutiny HTML, přidat nové metody pro třídu HtmlHelper reprezentována vlastnosti zobrazení Html.</span><span class="sxs-lookup"><span data-stu-id="c0420-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="c0420-164">Třída ve výpisu 3 přidá metodu rozšíření k `HtmlHelper` třídu s názvem `Label()`.</span><span class="sxs-lookup"><span data-stu-id="c0420-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="c0420-165">Existuje několik věcí, které byste si měli všimnout o této třídy.</span><span class="sxs-lookup"><span data-stu-id="c0420-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="c0420-166">Napřed si všimněte, že třída je statická třída.</span><span class="sxs-lookup"><span data-stu-id="c0420-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="c0420-167">Je nutné definovat rozšiřující metodu se statické třídě.</span><span class="sxs-lookup"><span data-stu-id="c0420-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="c0420-168">Za druhé, Všimněte si, že první parametr `Label()` metoda je před klíčové slovo `this`.</span><span class="sxs-lookup"><span data-stu-id="c0420-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="c0420-169">První parametr rozšiřující metody Určuje třídu, která rozšiřující metoda rozšiřuje.</span><span class="sxs-lookup"><span data-stu-id="c0420-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="c0420-170">**Výpis 3 – `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="c0420-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="c0420-171">Po vytvoření rozšiřující metodu a sestavení aplikace úspěšně, metoda rozšíření se zobrazí v Intellisense ve Visual Studio jako všechny ostatní metody třídy (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="c0420-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="c0420-172">Jediným rozdílem je tohoto rozšíření, které metody mají speciální symbol vedle sebe (ikonu šipky dolů).</span><span class="sxs-lookup"><span data-stu-id="c0420-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="c0420-173">[![Pomocí metody rozšíření Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c0420-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="c0420-174">**Obrázek 02**: použití metody rozšíření Html.Label() ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c0420-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="c0420-175">Upravené zobrazení indexu v informacích 4 používá metodu rozšíření Html.Label() k vykreslení všech jeho `<label>` značky.</span><span class="sxs-lookup"><span data-stu-id="c0420-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="c0420-176">**Část 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="c0420-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="c0420-177">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c0420-177">Summary</span></span>

<span data-ttu-id="c0420-178">V tomto kurzu jste zjistili, dva způsoby vytvoření vlastních pomocných rutin HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="c0420-179">Nejdřív jste zjistili, jak můžete vytvořit vlastní `Label()` pomocné rutiny HTML tak, že vytvoříte statická metoda, která vrátí hodnotu typu string.</span><span class="sxs-lookup"><span data-stu-id="c0420-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="c0420-180">Dále jste zjistili, jak můžete vytvořit vlastní `Label()` metody pomocné rutiny HTML, vytvořte rozšiřující metody pro `HtmlHelper` třídy.</span><span class="sxs-lookup"><span data-stu-id="c0420-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="c0420-181">V tomto kurzu můžu soustředit na vytváření velmi jednoduchý způsob pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="c0420-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="c0420-182">Uvědomte si, že pomocné rutiny HTML, může být složité, jak chcete.</span><span class="sxs-lookup"><span data-stu-id="c0420-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="c0420-183">Pomocné rutiny HTML, vykreslení formátovaného obsahu, jako je například stromová zobrazení, nabídky nebo tabulky databázových dat můžete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="c0420-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c0420-184">[Předchozí](asp-net-mvc-views-overview-cs.md)
> [další](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c0420-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
