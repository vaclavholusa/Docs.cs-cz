---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Použití zařízení Extender ovládacího prvku ColorPicker (C#) | Dokumentace Microsoftu
author: microsoft
description: ColorPicker je extenderu ASP.NET AJAX, která poskytuje funkce vybere barvu na straně klienta s uživatelským rozhraním v ovládacím prvku popup. Může být připojen k žádné ASP.NET...
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 308f56ecc1ff76ebdaea26e127a06687fcbcaba6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837584"
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="159d2-104">Použití zařízení Extender ovládacího prvku ColorPicker (C#)</span><span class="sxs-lookup"><span data-stu-id="159d2-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="159d2-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="159d2-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="159d2-106">ColorPicker je extenderu ASP.NET AJAX, která poskytuje funkce vybere barvu na straně klienta s uživatelským rozhraním v ovládacím prvku popup.</span><span class="sxs-lookup"><span data-stu-id="159d2-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="159d2-107">Může být připojen na libovolný ovládací prvek TextBox technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="159d2-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="159d2-108">Ho.</span><span class="sxs-lookup"><span data-stu-id="159d2-108">It.</span></span>


<span data-ttu-id="159d2-109">Cílem tohoto kurzu je vysvětlují, jak můžete použít zařízení extender ovládacího prvku Toolkit ColorPicker ovládacího prvku AJAX.</span><span class="sxs-lookup"><span data-stu-id="159d2-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="159d2-110">Extender ovládacího prvku ColorPicker zobrazí dialogové okno automaticky otevíraného okna, která umožňuje vybrat barvu.</span><span class="sxs-lookup"><span data-stu-id="159d2-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="159d2-111">ColorPicker je vhodný v každé byste chtěli poskytnout intuitivní uživatelské rozhraní pro uživatele pro výběr barvy.</span><span class="sxs-lookup"><span data-stu-id="159d2-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="159d2-112">Rozšíří ovládací prvek TextBox s zařízení Extender ovládacího prvku ColorPicker</span><span class="sxs-lookup"><span data-stu-id="159d2-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="159d2-113">Představte si například, že chcete vytvořit web, který umožňuje návštěvníkům vytváření přizpůsobených obchodních karet.</span><span class="sxs-lookup"><span data-stu-id="159d2-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="159d2-114">Návštěvníci můžete zadat text pro kartu firmy a vybrat barvu.</span><span class="sxs-lookup"><span data-stu-id="159d2-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="159d2-115">Na stránce technologie ASP.NET v informacích 1 obsahuje dva ovládací prvky textového pole s názvem txtCardText a txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="159d2-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="159d2-116">Při odeslání formuláře se zobrazí vybrané hodnoty (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="159d2-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="159d2-117">[![Jednoduchý formulář pro vytvoření karty firmy](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="159d2-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="159d2-118">**Obrázek 01**: jednoduchý formulář pro vytvoření vizitky ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="159d2-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="159d2-119">**Výpis 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="159d2-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="159d2-120">Formulář v nástrojích pro výpis 1 funguje, ale neposkytuje skvělé uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="159d2-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="159d2-121">Uživatel musí zadat barvu do textového pole.</span><span class="sxs-lookup"><span data-stu-id="159d2-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="159d2-122">Pokud uživatel požaduje specializované barvy – například právě správné odstín pea zelená - pak uživatel musí zjistit kód HTML bez pomoci.</span><span class="sxs-lookup"><span data-stu-id="159d2-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="159d2-123">Extender ovládacího prvku ColorPicker slouží k vytvoření lepší uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="159d2-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="159d2-124">ColorPicker zobrazí dialogové okno Barva, když se přesunete fokus na ovládací prvek textového pole (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="159d2-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="159d2-125">[![Extender ovládacího prvku ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="159d2-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="159d2-126">**Obrázek 02**: The extenderu ovládacího prvku ColorPicker ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="159d2-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="159d2-127">Je třeba provést dva kroky pro použití s formulář v nástrojích pro výpis 1 extender ovládacího prvku ColorPicker:</span><span class="sxs-lookup"><span data-stu-id="159d2-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="159d2-128">Přidání ovládacího prvku ScriptManager na stránce</span><span class="sxs-lookup"><span data-stu-id="159d2-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="159d2-129">Extender ovládacího prvku ColorPicker přidat na stránku</span><span class="sxs-lookup"><span data-stu-id="159d2-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="159d2-130">Před použitím ColorPicker, je nutné přidat ovládací prvek ScriptManager na stránku.</span><span class="sxs-lookup"><span data-stu-id="159d2-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="159d2-131">Je vhodné místo pro přidání ScriptManager přímo pod levou serverové &lt;formuláře&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="159d2-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="159d2-132">Prvek ScriptManager na stránku můžete přetáhnout z panelu nástrojů (prvek ScriptManager se nachází na kartě Rozšíření AJAX).</span><span class="sxs-lookup"><span data-stu-id="159d2-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="159d2-133">Alternativně můžete zadat následující značku do zobrazení zdroje pod počáteční značka pro formulář na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="159d2-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="159d2-134">&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="159d2-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="159d2-135">Nejjednodušší způsob, jak přidat na stránku – extender ovládacího prvku ColorPicker je v zobrazení Návrh.</span><span class="sxs-lookup"><span data-stu-id="159d2-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="159d2-136">Pokud myší najedete myší txtCardColor textové pole, inteligentní úloh možnost bude nabídnuta povoluje můžete přidat zařízení extender (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="159d2-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="159d2-137">Pokud vyberete tuto možnost, zobrazí se Průvodce zařízení Extender (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="159d2-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="159d2-138">[![Přidání zařízení extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="159d2-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="159d2-139">**Obrázek 03**: přidání zařízení extender ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="159d2-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="159d2-140">[![Extender ovládacího prvku pomocí Průvodce rozšiřující výběr](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="159d2-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="159d2-141">**Obrázek 04**: výběr – extender ovládacího prvku pomocí Průvodce zařízení Extender ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="159d2-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="159d2-142">Můžete si vybrat zařízení extender ColorPicker rozšířit txtCardColor textové pole s ColorPicker zařízení extender.</span><span class="sxs-lookup"><span data-stu-id="159d2-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="159d2-143">Klikněte na tlačítko OK zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="159d2-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="159d2-144">Po provedení těchto změn zdroje stránky vypadá výpis 2.</span><span class="sxs-lookup"><span data-stu-id="159d2-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="159d2-145">Výpis 2 - CreateCard.aspx (s ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="159d2-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="159d2-146">Všimněte si, že stránka nyní obsahuje ColorPickerExtender ovládací prvek, který se zobrazí přímo pod txtCardColor TextBox – ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="159d2-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="159d2-147">Ovládací prvek ColorPickerExtender rozšiřuje txtCardColor ovládacího prvku tak, aby zobrazil dialogové okno Výběr barvy.</span><span class="sxs-lookup"><span data-stu-id="159d2-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="159d2-148">Pomocí tlačítka Spustit dialogové okno Výběr barvy</span><span class="sxs-lookup"><span data-stu-id="159d2-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="159d2-149">Zařízení extender ColorPicker podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="159d2-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="159d2-150">PopupButtonId - ID na stránce, která způsobí, že dialogové okno Výběr barvy se zobrazí tlačítko.</span><span class="sxs-lookup"><span data-stu-id="159d2-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="159d2-151">PopupPosition – pozice relativně k cílovému ovládacímu prvku dialogového okna pro výběr barvy.</span><span class="sxs-lookup"><span data-stu-id="159d2-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="159d2-152">Možné hodnoty jsou absolutní, System Center, BottomLeft, BottomRight, TopLeft, TopRight vpravo a vlevo (výchozí hodnota je BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="159d2-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="159d2-153">SampleControlId - ID pro ovládací prvek zobrazující vybranou barvu.</span><span class="sxs-lookup"><span data-stu-id="159d2-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="159d2-154">SelectedColor – Počáteční barva zvolila ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="159d2-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="159d2-155">Tyto vlastnosti můžete přizpůsobit, jak je zobrazeno dialogové okno Výběr barvy a jak se zobrazí vybranou barvu.</span><span class="sxs-lookup"><span data-stu-id="159d2-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="159d2-156">Na stránce v informacích 3 ukazuje, jak můžete používat některé z těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="159d2-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="159d2-157">**Výpis 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="159d2-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="159d2-158">Na stránce v informacích 3 zahrnuje výběr barvy tlačítka (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="159d2-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="159d2-159">Po kliknutí na toto tlačítko, zobrazí se dialogové okno Výběr barvy vyšší než textovém poli.</span><span class="sxs-lookup"><span data-stu-id="159d2-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="159d2-160">Je-li vybrat barvu z tohoto dialogového okna vybraná barva zobrazí jako barva pozadí lblSample ovládacího prvku popisku.</span><span class="sxs-lookup"><span data-stu-id="159d2-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="159d2-161">Vlastnost ColorPicker PopupButtonID je slouží k přidružení zařízení extender ColorPicker tlačítko Vybrat barvu.</span><span class="sxs-lookup"><span data-stu-id="159d2-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="159d2-162">Pokud zadáte hodnotu pro vlastnost PopupButtonID, dialogové okno Výběr barvy už se zobrazí, když cílový ovládací prvek má fokus.</span><span class="sxs-lookup"><span data-stu-id="159d2-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="159d2-163">Musíte kliknout na tlačítko pro zobrazení dialogového okna.</span><span class="sxs-lookup"><span data-stu-id="159d2-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="159d2-164">Vlastnost SampleControlID je použito k přidružení ovládací prvek, který zobrazuje vybrané barvy s ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="159d2-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="159d2-165">Barva pozadí tohoto ovládacího prvku ColorPicker změní na aktuálně vybraná barva.</span><span class="sxs-lookup"><span data-stu-id="159d2-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="159d2-166">[![Zobrazení dialogového okna pro výběr barvy s tlačítkem](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="159d2-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="159d2-167">**Obrázek 05**: zobrazení dialogového okna pro výběr barvy s tlačítkem ([kliknutím ji zobrazíte obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="159d2-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="159d2-168">Souhrn</span><span class="sxs-lookup"><span data-stu-id="159d2-168">Summary</span></span>

<span data-ttu-id="159d2-169">V tomto kurzu jste zjistili, jak použít zařízení extender ovládacího prvku ColorPicker zobrazíte dialogové okno Výběr barvy automaticky otevíraného okna.</span><span class="sxs-lookup"><span data-stu-id="159d2-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="159d2-170">Nejprve jsme prozkoumat, jak můžete zobrazit dialogové okno když fokus se přesune na ovládací prvek textového pole.</span><span class="sxs-lookup"><span data-stu-id="159d2-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="159d2-171">Dále jste zjistili, jak vytvořit tlačítko, které při kliknutí na tlačítko se zobrazí dialogové okno Výběr barvy.</span><span class="sxs-lookup"><span data-stu-id="159d2-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="159d2-172">Next</span><span class="sxs-lookup"><span data-stu-id="159d2-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
