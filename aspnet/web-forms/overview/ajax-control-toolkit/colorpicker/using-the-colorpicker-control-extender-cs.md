---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: "Pomocí rozšiřujícího objektu ColorPicker řízení (C#) | Microsoft Docs"
author: microsoft
description: "ColorPicker je prvku ASP.NET AJAX rozšiřujícího objektu, která poskytuje funkci Barva výdej straně klienta pomocí uživatelského rozhraní v ovládacím prvku místní. Je možné připojit k žádné ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3cde9552e8aecd5e7e651a825902fb79ae108c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-colorpicker-control-extender-c"></a><span data-ttu-id="b4677-104">Pomocí rozšiřujícího objektu ColorPicker řízení (C#)</span><span class="sxs-lookup"><span data-stu-id="b4677-104">Using the ColorPicker Control Extender (C#)</span></span>
====================
<span data-ttu-id="b4677-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b4677-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b4677-106">ColorPicker je prvku ASP.NET AJAX rozšiřujícího objektu, která poskytuje funkci Barva výdej straně klienta pomocí uživatelského rozhraní v ovládacím prvku místní.</span><span class="sxs-lookup"><span data-stu-id="b4677-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="b4677-107">Se lze připojit k žádné ASP.NET TextBox – ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="b4677-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="b4677-108">Ho.</span><span class="sxs-lookup"><span data-stu-id="b4677-108">It.</span></span>


<span data-ttu-id="b4677-109">Cílem tohoto kurzu je vysvětlují, jak můžete použít rozšiřujícího objektu řízení Toolkit ColorPicker řízení AJAX.</span><span class="sxs-lookup"><span data-stu-id="b4677-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="b4677-110">Rozšiřujícího objektu řízení ColorPicker zobrazí automaticky otevřeném okně. dialog, který vám umožní vybrat barvu.</span><span class="sxs-lookup"><span data-stu-id="b4677-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="b4677-111">ColorPicker je užitečné, kdykoli budete chtít poskytnout intuitivní uživatelské rozhraní pro uživatele a vybrat barvu.</span><span class="sxs-lookup"><span data-stu-id="b4677-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="b4677-112">Rozšíření ovládacího prvku textového pole s rozšiřujícího objektu ColorPicker ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="b4677-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="b4677-113">Představte si například, že chcete vytvořit web, který umožňuje uživatelům serveru k vytvoření vlastní karty firmy.</span><span class="sxs-lookup"><span data-stu-id="b4677-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="b4677-114">Návštěvníky můžete zadejte text pro vizitky a vybrat barvu.</span><span class="sxs-lookup"><span data-stu-id="b4677-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="b4677-115">Stránka ASP.NET v výpis 1 obsahuje dva ovládací prvky textové pole s názvem txtCardText a txtCardColor.</span><span class="sxs-lookup"><span data-stu-id="b4677-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="b4677-116">Při odesílání formuláře, vybrané hodnoty jsou zobrazeny (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="b4677-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="b4677-117">[![Jednoduchý formulář pro vytvoření vizitky](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b4677-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)</span></span>

<span data-ttu-id="b4677-118">**Obrázek 01**: jednoduchý formulář pro vytvoření vizitky ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b4677-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image2.png))</span></span>


<span data-ttu-id="b4677-119">**Výpis 1 - CreateCard.aspx**</span><span class="sxs-lookup"><span data-stu-id="b4677-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

<span data-ttu-id="b4677-120">Formuláře v výpis 1 funguje, ale neposkytuje vysoký výkon uživatele.</span><span class="sxs-lookup"><span data-stu-id="b4677-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="b4677-121">Do textového pole Zadejte barvu, která má uživatel.</span><span class="sxs-lookup"><span data-stu-id="b4677-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="b4677-122">Pokud uživatel chce specializované barev – například právě správné odstín pea zelená - pak uživatel musí rozmyslete si kód HTML bez pomoci.</span><span class="sxs-lookup"><span data-stu-id="b4677-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="b4677-123">Rozšiřujícího objektu řízení ColorPicker slouží k vytvoření lepší uživatelské prostředí.</span><span class="sxs-lookup"><span data-stu-id="b4677-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="b4677-124">ColorPicker zobrazí dialogové okno barvy, když se přesunout fokus na ovládací prvek textové pole (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="b4677-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="b4677-125">[![Ovládací prvek ColorPicker rozšiřujícího objektu](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b4677-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)</span></span>

<span data-ttu-id="b4677-126">**Obrázek 02**: rozšiřujícího objektu ColorPicker ovládací prvek ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b4677-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image4.png))</span></span>


<span data-ttu-id="b4677-127">Je potřeba provést dva kroky pro použití rozšíření řízení ColorPicker s formuláři v výpis 1:</span><span class="sxs-lookup"><span data-stu-id="b4677-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="b4677-128">Přidání ovládacího prvku ScriptManager na stránku</span><span class="sxs-lookup"><span data-stu-id="b4677-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="b4677-129">Přidání rozšiřujícího objektu řízení ColorPicker na stránku</span><span class="sxs-lookup"><span data-stu-id="b4677-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="b4677-130">Než budete moct použít ColorPicker, je nutné přidat na stránku ovládací prvek ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="b4677-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="b4677-131">Je vhodná k přidání prvek ScriptManager přímo pod serverovou otevírání &lt;formuláře&gt; značky.</span><span class="sxs-lookup"><span data-stu-id="b4677-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="b4677-132">Můžete přetáhnout prvek ScriptManager na stránce z panelu nástrojů (prvek ScriptManager se nachází na kartě Rozšíření AJAX).</span><span class="sxs-lookup"><span data-stu-id="b4677-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="b4677-133">Alternativně můžete zadat následující značka do zobrazení zdroje pod počáteční značka formuláře na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="b4677-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="b4677-134">&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="b4677-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="b4677-135">V návrhovém zobrazení je nejjednodušší způsob, jak přidat rozšiřujícího objektu řízení ColorPicker na stránku.</span><span class="sxs-lookup"><span data-stu-id="b4677-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="b4677-136">Pokud umístěte ukazatel myši nad txtCardColor textové pole, možnost Inteligentní úloh, zobrazí se povoluje můžete přidat rozšiřujícího objektu (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="b4677-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="b4677-137">Pokud vyberete tuto možnost, zobrazí se Průvodce rozšiřujícího objektu (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="b4677-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="b4677-138">[![Přidání extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b4677-138">[![Adding an extender](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)</span></span>

<span data-ttu-id="b4677-139">**Obrázek 03**: Přidání extender ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b4677-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="b4677-140">[![Výběr ovládacího prvku extender pomocí Průvodce rozšiřujícího objektu](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b4677-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="b4677-141">**Obrázek 04**: výběr extender ovládacího prvku pomocí Průvodce rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="b4677-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image8.png))</span></span>


<span data-ttu-id="b4677-142">Můžete si vybrat rozšiřujícího objektu ColorPicker rozšířit txtCardColor textové pole s ColorPicker rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="b4677-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="b4677-143">Kliknutím na OK zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4677-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="b4677-144">Po provedení těchto změn zdroj pro stránku vypadá jako výpis 2.</span><span class="sxs-lookup"><span data-stu-id="b4677-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="b4677-145">Výpis 2 - CreateCard.aspx (s ColorPicker)</span><span class="sxs-lookup"><span data-stu-id="b4677-145">Listing 2 - CreateCard.aspx (with ColorPicker)</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

<span data-ttu-id="b4677-146">Všimněte si, zda tato stránka obsahuje teď ColorPickerExtender ovládací prvek, který se zobrazí pod txtCardColor TextBox – ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="b4677-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="b4677-147">Ovládací prvek ColorPickerExtender rozšiřuje ovládacího prvku txtCardColor tak, aby se zobrazí dialogové okno pro výběr barev.</span><span class="sxs-lookup"><span data-stu-id="b4677-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="b4677-148">Pomocí tlačítka Spustit dialogové okno pro výběr barev</span><span class="sxs-lookup"><span data-stu-id="b4677-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="b4677-149">Rozšiřujícího objektu ColorPicker podporuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b4677-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="b4677-150">PopupButtonId - ID tlačítka na stránku, která způsobí, že dialogového okna pro výběr barev zobrazí.</span><span class="sxs-lookup"><span data-stu-id="b4677-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="b4677-151">PopupPosition - pozice relativně k cílový ovládací prvek dialogového okna pro výběr barev.</span><span class="sxs-lookup"><span data-stu-id="b4677-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="b4677-152">Možné hodnoty jsou absolutní, Center, BottomLeft, BottomRight, TopLeft, TopRight práva a doleva (výchozí hodnota je BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="b4677-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="b4677-153">SampleControlId - ID ovládacího prvku, který zobrazí vybrané barvy.</span><span class="sxs-lookup"><span data-stu-id="b4677-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="b4677-154">SelectedColor – počáteční barvu vybraná ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="b4677-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="b4677-155">Tyto vlastnosti můžete přizpůsobit zobrazení dialogového okna pro výběr barvy a zobrazení vybrané barvy.</span><span class="sxs-lookup"><span data-stu-id="b4677-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="b4677-156">Stránka v výpis 3 znázorňuje, jak můžete použít několik z těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="b4677-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="b4677-157">**Výpis 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="b4677-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

<span data-ttu-id="b4677-158">Na stránku výpis 3 zahrnuje barvu vyberte tlačítko (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="b4677-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="b4677-159">Kliknutí na toto tlačítko se zobrazí dialogové okno pro výběr barev nad textové pole.</span><span class="sxs-lookup"><span data-stu-id="b4677-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="b4677-160">Pokud vyberete barvu, která z tohoto dialogového okna vybrané barvy zobrazí jako barvu pozadí lblSample popisek – ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="b4677-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="b4677-161">Vlastnost ColorPicker PopupButtonID slouží k Barva vyberte tlačítko přidružit ColorPicker rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="b4677-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="b4677-162">Pokud zadáte hodnotu pro vlastnost PopupButtonID, dialogové okno pro výběr barev už se zobrazí, když má právě fokus, cílový ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="b4677-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="b4677-163">Musíte kliknout na tlačítko zobrazit dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="b4677-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="b4677-164">Vlastnost SampleControlID je použito k přidružení ovládacího prvku, který zobrazí vybrané barvy s ColorPicker.</span><span class="sxs-lookup"><span data-stu-id="b4677-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="b4677-165">ColorPicker změní barvu pozadí tohoto ovládacího prvku na aktuálně vybrané barvy.</span><span class="sxs-lookup"><span data-stu-id="b4677-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="b4677-166">[![Zobrazení dialogového okna pro výběr barvy s tlačítkem na](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="b4677-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)</span></span>

<span data-ttu-id="b4677-167">**Obrázek 05**: zobrazení dialogového okna pro výběr barvy s tlačítkem na ([Kliknutím zobrazit obrázek v plné velikosti](using-the-colorpicker-control-extender-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="b4677-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-cs/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="b4677-168">Souhrn</span><span class="sxs-lookup"><span data-stu-id="b4677-168">Summary</span></span>

<span data-ttu-id="b4677-169">V tomto kurzu jste zjistili, jak používat rozšiřujícího objektu řízení ColorPicker zobrazíte dialogové okno pro výběr barev místní.</span><span class="sxs-lookup"><span data-stu-id="b4677-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="b4677-170">Nejdřív jsme se zaměřili jak můžete zobrazit dialogové okno když fokus se přesune do ovládacího prvku textového pole.</span><span class="sxs-lookup"><span data-stu-id="b4677-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="b4677-171">V dalším kroku jste zjistili, jak vytvořit tlačítko, které se zobrazí dialogové okno pro výběr barev, když se po kliknutí na tlačítko.</span><span class="sxs-lookup"><span data-stu-id="b4677-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b4677-172">Další</span><span class="sxs-lookup"><span data-stu-id="b4677-172">Next</span></span>](using-the-colorpicker-control-extender-vb.md)
