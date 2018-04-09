---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Vytváření Pomocníci vlastní HTML (C#) | Microsoft Docs
author: microsoft
description: Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní pomocné rutiny HTML, který můžete použít v rámci zobrazení v rozhraní MVC. Díky pomocné rutiny HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: ebc9aa2aa8dbc02dc01833d671c3bfd19141ba74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-html-helpers-c"></a><span data-ttu-id="facd3-104">Vytváření Pomocníci vlastní HTML (C#)</span><span class="sxs-lookup"><span data-stu-id="facd3-104">Creating Custom HTML Helpers (C#)</span></span>
====================
<span data-ttu-id="facd3-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="facd3-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="facd3-106">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="facd3-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> <span data-ttu-id="facd3-107">Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní pomocné rutiny HTML, který můžete použít v rámci zobrazení v rozhraní MVC.</span><span class="sxs-lookup"><span data-stu-id="facd3-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="facd3-108">S využitím pomocné rutiny HTML, můžete snížit množství zdlouhavé zadáním značky HTML, musí provést k vytvoření standardní stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>


<span data-ttu-id="facd3-109">Cílem tohoto kurzu je ukazují, jak můžete vytvořit vlastní pomocné rutiny HTML, který můžete použít v rámci zobrazení v rozhraní MVC.</span><span class="sxs-lookup"><span data-stu-id="facd3-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="facd3-110">S využitím pomocné rutiny HTML, můžete snížit množství zdlouhavé zadáním značky HTML, musí provést k vytvoření standardní stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="facd3-111">V první části tohoto kurzu I popisují některé z existujících pomocné objekty HTML součástí rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="facd3-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="facd3-112">V dalším kroku I popisují dvě metody vytváření vlastní pomocné rutiny HTML: I vysvětlují, jak vytvořit vlastní pomocné rutiny HTML a vytváření statickou metodu metody rozšíření.</span><span class="sxs-lookup"><span data-stu-id="facd3-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a static method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="facd3-113">Principy pomocné objekty HTML</span><span class="sxs-lookup"><span data-stu-id="facd3-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="facd3-114">Pomocné rutiny HTML je právě metoda, která vrací řetězec.</span><span class="sxs-lookup"><span data-stu-id="facd3-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="facd3-115">Řetězec může představovat jakýkoli typ obsahu, které chcete.</span><span class="sxs-lookup"><span data-stu-id="facd3-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="facd3-116">Pomocné rutiny HTML můžete například použít k vykreslení standardní značky HTML jako HTML `<input>` a `<img>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="facd3-117">Pomocné rutiny HTML také můžete použít k vykreslení složitější obsah, jako jsou karty pruhu nebo tabulky HTML dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="facd3-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="facd3-118">Rozhraní ASP.NET MVC zahrnuje následující sadu standardní pomocné rutiny HTML (Toto není úplný seznam):</span><span class="sxs-lookup"><span data-stu-id="facd3-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="facd3-119">Html.ActionLink()</span><span class="sxs-lookup"><span data-stu-id="facd3-119">Html.ActionLink()</span></span>
- <span data-ttu-id="facd3-120">Html.BeginForm()</span><span class="sxs-lookup"><span data-stu-id="facd3-120">Html.BeginForm()</span></span>
- <span data-ttu-id="facd3-121">Html.CheckBox()</span><span class="sxs-lookup"><span data-stu-id="facd3-121">Html.CheckBox()</span></span>
- <span data-ttu-id="facd3-122">Html.DropDownList()</span><span class="sxs-lookup"><span data-stu-id="facd3-122">Html.DropDownList()</span></span>
- <span data-ttu-id="facd3-123">Html.EndForm()</span><span class="sxs-lookup"><span data-stu-id="facd3-123">Html.EndForm()</span></span>
- <span data-ttu-id="facd3-124">Html.Hidden()</span><span class="sxs-lookup"><span data-stu-id="facd3-124">Html.Hidden()</span></span>
- <span data-ttu-id="facd3-125">Html.ListBox()</span><span class="sxs-lookup"><span data-stu-id="facd3-125">Html.ListBox()</span></span>
- <span data-ttu-id="facd3-126">Html.Password()</span><span class="sxs-lookup"><span data-stu-id="facd3-126">Html.Password()</span></span>
- <span data-ttu-id="facd3-127">Html.RadioButton()</span><span class="sxs-lookup"><span data-stu-id="facd3-127">Html.RadioButton()</span></span>
- <span data-ttu-id="facd3-128">Html.TextArea()</span><span class="sxs-lookup"><span data-stu-id="facd3-128">Html.TextArea()</span></span>
- <span data-ttu-id="facd3-129">Html.TextBox()</span><span class="sxs-lookup"><span data-stu-id="facd3-129">Html.TextBox()</span></span>

<span data-ttu-id="facd3-130">Představte si třeba formuláře v výpis 1.</span><span class="sxs-lookup"><span data-stu-id="facd3-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="facd3-131">Tento formulář je vykreslen pomocí dva standardní pomocné objekty HTML (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="facd3-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="facd3-132">Tento formulář používá `Html.BeginForm()` a `Html.TextBox()` pomocné metody pro vykreslení jednoduché formuláře HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods to render a simple HTML form.</span></span>


<span data-ttu-id="facd3-133">[![Stránka vykresluje se pomocné objekty HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="facd3-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)</span></span>

<span data-ttu-id="facd3-134">**Obrázek 01**: stránka vykresluje se pomocné objekty HTML ([Kliknutím zobrazit obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="facd3-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image3.png))</span></span>


<span data-ttu-id="facd3-135">**Výpis 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="facd3-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

<span data-ttu-id="facd3-136">Html.BeginForm() Pomocná metoda se používá k vytvoření HTML otevřením a uzavření `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-136">The Html.BeginForm() Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="facd3-137">Všimněte si, že `Html.BeginForm()` metoda je volána v rámci pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="facd3-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="facd3-138">Pomocí příkazu zajišťuje, že `<form>` značky zavřena na konci použití bloku.</span><span class="sxs-lookup"><span data-stu-id="facd3-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="facd3-139">Pokud dáváte přednost, místo vytvoření použití bloku, můžete volat metodu Html.EndForm() Helper zavřete `<form>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="facd3-140">Použít libovolný přístup k vytváření počáteční a uzavírání `<form>` značky, které by vám nejvíce intuitivní.</span><span class="sxs-lookup"><span data-stu-id="facd3-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="facd3-141">`Html.TextBox()` Pomocné metody se používají v výpis 1 k vykreslení HTML `<input>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="facd3-142">Pokud vyberete zobrazení Zdroj v prohlížeči zobrazí zdroji HTML ve výpisu 2.</span><span class="sxs-lookup"><span data-stu-id="facd3-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="facd3-143">Všimněte si, že zdroj obsahuje standardní značky HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="facd3-144">Všimněte si, že `Html.TextBox()`-HTML Helper je vykreslen pomocí `<%= %>` značky místo `<% %>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="facd3-145">Pokud nezadáte znaménku rovná, pak nic získá zobrazí v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="facd3-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="facd3-146">Rozhraní ASP.NET MVC obsahuje malou sadu pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="facd3-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="facd3-147">S největší pravděpodobností musíte rozšířit rozhraní MVC s vlastní pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="facd3-148">Ve zbývající části tohoto kurzu další dvě metody vytváření vlastní pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="facd3-149">**Výpis 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="facd3-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a><span data-ttu-id="facd3-150">Vytváření Pomocníci HTML pomocí statických metod</span><span class="sxs-lookup"><span data-stu-id="facd3-150">Creating HTML Helpers with Static Methods</span></span>

<span data-ttu-id="facd3-151">Nejjednodušší způsob, jak vytvořit nový pomocné rutiny HTML, je vytvoření statickou metodu, která vrací řetězec.</span><span class="sxs-lookup"><span data-stu-id="facd3-151">The easiest way to create a new HTML Helper is to create a static method that returns a string.</span></span> <span data-ttu-id="facd3-152">Představte si například, že se rozhodnete vytvořit nový pomocné rutiny HTML, který vykreslí HTML `<label>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="facd3-153">Třída v výpis 2 můžete použít k vykreslení `<label>` .</span><span class="sxs-lookup"><span data-stu-id="facd3-153">You can use the class in Listing 2 to render a `<label>` .</span></span>

<span data-ttu-id="facd3-154">**Výpis 2 – `Helpers\LabelHelper.cs`**</span><span class="sxs-lookup"><span data-stu-id="facd3-154">**Listing 2 – `Helpers\LabelHelper.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

<span data-ttu-id="facd3-155">Není co speciální o třídy ve výpisu 2.</span><span class="sxs-lookup"><span data-stu-id="facd3-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="facd3-156">`Label()` Metoda jednoduše vrátí řetězec.</span><span class="sxs-lookup"><span data-stu-id="facd3-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="facd3-157">Používá upravené zobrazení indexu v výpis 3 `LabelHelper` k vykreslení HTML `<label>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="facd3-158">Všimněte si, že zobrazení zahrnuje `<%@ imports %>` direktiva, která importuje `Application1.Helpers` oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="facd3-158">Notice that the view includes an `<%@ imports %>` directive that imports the `Application1.Helpers` namespace.</span></span>

<span data-ttu-id="facd3-159">**Výpis 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="facd3-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="facd3-160">Vytváření Pomocníci HTML pomocí metod rozšíření</span><span class="sxs-lookup"><span data-stu-id="facd3-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="facd3-161">Pokud chcete vytvořit pomocné rutiny HTML, které fungují stejně jako standardní pomocné objekty HTML součástí rozhraní ASP.NET MVC, pak je potřeba vytvořit rozšiřující metody.</span><span class="sxs-lookup"><span data-stu-id="facd3-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="facd3-162">Rozšiřující metody umožňují přidat nové metody do existující třídy.</span><span class="sxs-lookup"><span data-stu-id="facd3-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="facd3-163">Při vytváření metody pomocné rutiny HTML, přidáte nové metody pro třídu HtmlHelper reprezentována vlastnost Html zobrazení.</span><span class="sxs-lookup"><span data-stu-id="facd3-163">When creating an HTML Helper method, you add new methods to the HtmlHelper class represented by a view's Html property.</span></span>

<span data-ttu-id="facd3-164">Třídy ve výpisu 3 přidá metody rozšíření pro `HtmlHelper` třídu s názvem `Label()`.</span><span class="sxs-lookup"><span data-stu-id="facd3-164">The class in Listing 3 adds an extension method to the `HtmlHelper` class named `Label()`.</span></span> <span data-ttu-id="facd3-165">Existuje několik věcí, které byste měli zaznamenat o tuto třídu.</span><span class="sxs-lookup"><span data-stu-id="facd3-165">There are a couple of things that you should notice about this class.</span></span> <span data-ttu-id="facd3-166">První Všimněte si, že třída je statická třída.</span><span class="sxs-lookup"><span data-stu-id="facd3-166">First, notice that the class is a static class.</span></span> <span data-ttu-id="facd3-167">Je nutné zadat metody rozšíření pomocí statické třídy.</span><span class="sxs-lookup"><span data-stu-id="facd3-167">You must define an extension method with a static class.</span></span>

<span data-ttu-id="facd3-168">Druhý, Všimněte si, že první parametr `Label()` metoda je zadáno klíčové slovo `this`.</span><span class="sxs-lookup"><span data-stu-id="facd3-168">Second, notice that the first parameter of the `Label()` method is preceded by the keyword `this`.</span></span> <span data-ttu-id="facd3-169">První parametr metody rozšíření Určuje třídu, která rozšíření metoda rozšiřuje.</span><span class="sxs-lookup"><span data-stu-id="facd3-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="facd3-170">**Výpis 3 – `Helpers\LabelExtensions.cs`**</span><span class="sxs-lookup"><span data-stu-id="facd3-170">**Listing 3 – `Helpers\LabelExtensions.cs`**</span></span>

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

<span data-ttu-id="facd3-171">Po vytvoření metody rozšíření a úspěšně sestavit aplikaci, metoda rozšíření se zobrazí v aplikaci Visual Studio Intellisense jako všechny ostatní metody třídy (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="facd3-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="facd3-172">Jediným rozdílem je, že rozšiřující metody zobrazují se speciální symbolem vedle sebe (ikonu šipky dolů).</span><span class="sxs-lookup"><span data-stu-id="facd3-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>


<span data-ttu-id="facd3-173">[![Pomocí metody rozšíření Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="facd3-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)</span></span>

<span data-ttu-id="facd3-174">**Obrázek 02**: použití metody rozšíření Html.Label() ([Kliknutím zobrazit obrázek v plné velikosti](creating-custom-html-helpers-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="facd3-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-cs/_static/image6.png))</span></span>


<span data-ttu-id="facd3-175">Upravené zobrazení indexu v výpis 4 používá metodu Html.Label() rozšíření pro vykreslení všechny jeho `<label>` značky.</span><span class="sxs-lookup"><span data-stu-id="facd3-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its `<label>` tags.</span></span>

<span data-ttu-id="facd3-176">**Výpis 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="facd3-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="facd3-177">Souhrn</span><span class="sxs-lookup"><span data-stu-id="facd3-177">Summary</span></span>

<span data-ttu-id="facd3-178">V tomto kurzu jste se dozvěděli dvě metody vytváření vlastní pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="facd3-179">Nejdříve jste zjistili, jak vytvořit vlastní `Label()` pomocné rutiny HTML tak, že vytvoříte statickou metodu, která vrací řetězec.</span><span class="sxs-lookup"><span data-stu-id="facd3-179">First, you learned how to create a custom `Label()` HTML Helper by creating a static method that returns a string.</span></span> <span data-ttu-id="facd3-180">V dalším kroku jste zjistili, jak vytvořit vlastní `Label()` metodu HTML Helper vytvořením metody rozšíření na `HtmlHelper` třídy.</span><span class="sxs-lookup"><span data-stu-id="facd3-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="facd3-181">V tomto kurzu I zaměřuje na budování metodu velmi jednoduché pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="facd3-182">Uvědomte si, že může být složité, protože chcete, aby pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="facd3-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="facd3-183">Můžete vytvořit pomocné rutiny HTML, která vykreslit bohaté obsahu například stromové zobrazení, nabídky nebo tabulky dat z databáze.</span><span class="sxs-lookup"><span data-stu-id="facd3-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="facd3-184">[Předchozí](asp-net-mvc-views-overview-cs.md)
> [další](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span><span class="sxs-lookup"><span data-stu-id="facd3-184">[Previous](asp-net-mvc-views-overview-cs.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-cs.md)</span></span>
