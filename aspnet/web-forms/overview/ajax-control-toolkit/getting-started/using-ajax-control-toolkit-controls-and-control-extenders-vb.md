---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Pomocí ovládacích prvků AJAX Control Toolkit ovládacích prvků a extenderů (VB) | Dokumentace Microsoftu
author: microsoft
description: Informace o přidání ovládacích prvků AJAX Control Toolkit a zařízení Extender na stránky ASP.NET.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: f1cf40ec3ba299ee2702258a93aa1192a2112e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752212"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="e5ebe-103">Pomocí ovládacích prvků AJAX Control Toolkit ovládacích prvků a extenderů (VB)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="e5ebe-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e5ebe-105">Informace o přidání ovládacích prvků AJAX Control Toolkit a zařízení Extender na stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="e5ebe-106">Sada nástrojů AJAX Control Toolkit obsahuje sadu ovládacích prvků a extenderů ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="e5ebe-107">V tomto kurzu (BRIEF) se dozvíte, jak přidat ovládací prvky a ovládací prvek extenderů ke stránce ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e5ebe-108">Pokyny k instalaci sadou nástrojů AJAX Control Toolkit a sadou nástrojů AJAX Control Toolkit přidává do panelu nástrojů Visual Studio nebo Visual Web Developer naleznete v kurzu [Začínáme se sadou nástrojů AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span><span class="sxs-lookup"><span data-stu-id="e5ebe-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="e5ebe-109">Pomocí ovládacích prvků AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="e5ebe-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="e5ebe-110">Ovládací prvek AJAX Control Toolkit funguje stejně jako normální ovládací prvek technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="e5ebe-111">Přetáhněte ovládací prvek z panelu nástrojů na stránce ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="e5ebe-112">Přidejte ovládací prvek na stránce v zobrazení návrhu nebo v zobrazení zdroje.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="e5ebe-113">Jestli používáte ovládací prvky z sadou nástrojů AJAX Control Toolkit je speciální požadavků.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="e5ebe-114">Stránka musí obsahovat ovládací prvek ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="e5ebe-115">Ovládací prvek ScriptManager je zodpovědná za všechny nezbytné JavaScript vyžaduje ovládacích prvků AJAX Control Toolkit včetně.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="e5ebe-116">Například karta sada nástrojů AJAX Control Toolkit obsahuje ovládací prvek s názvem ovládací prvek editoru.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="e5ebe-117">Tento ovládací prvek zobrazuje bohaté možnosti editoru jazyka HTML.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="e5ebe-118">Postupujte podle těchto kroků přidejte ovládací prvek editoru na stránku:</span><span class="sxs-lookup"><span data-stu-id="e5ebe-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="e5ebe-119">Vytvořit novou stránku ASP.NET s názvem ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="e5ebe-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="e5ebe-120">Vyberte ovládací prvek ScriptManager from beneath na kartě Rozšíření AJAX v sadě nástrojů a přetáhněte ovládací prvek na stránce.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="e5ebe-121">Vyberte ovládací prvek editoru from beneath sadou nástrojů AJAX Control Toolkit kartu na panelu nástrojů a přetáhněte ovládací prvek na stránce (viz obrázek 1).</span><span class="sxs-lookup"><span data-stu-id="e5ebe-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="e5ebe-122">Návrhář by měl vypadat jako na obrázku 2.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="e5ebe-123">Spustit web tak, že vyberete možnost nabídky **ladit, spustit ladění** nebo stisknutí klávesy F5.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="e5ebe-124">Měli byste vidět stránku na obrázku 3.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="e5ebe-125">[![Vyberte ovládací prvek editoru HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="e5ebe-126">**Obrázek 01**: vyberete ovládací prvek editoru HTML ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="e5ebe-127">[![Návrhář Visual Studio ovládacímu prvku ScriptManager a úpravy](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="e5ebe-128">**Obrázek 02**: Návrhář Visual Studio ovládacímu prvku ScriptManager a úpravy ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="e5ebe-129">[![Na stránce DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="e5ebe-130">**Obrázek 03**: The DisplayEditor.aspx stránky ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="e5ebe-131">Použití jazyka AJAX Toolkit ovládací prvek extenderů</span><span class="sxs-lookup"><span data-stu-id="e5ebe-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="e5ebe-132">Sada nástrojů AJAX Control Toolkit obsahuje také ovládací prvek extenderů.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="e5ebe-133">Jak název napovídá, – extender ovládacího prvku rozšiřuje funkce existujícího ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="e5ebe-134">Například zařízení extender ConfirmButton ovládací prvek rozšiřuje standardní ovládací prvek tlačítko technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="e5ebe-135">Zařízení extender změní chování s ovládací prvek tlačítka tak, aby na tomto tlačítku zobrazí potvrzovací dialogové okno po kliknutí.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="e5ebe-136">Extender ovládacího prvku, stejně jako ovládací prvek AJAX Control Toolkit vyžaduje ovládací prvek ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="e5ebe-137">Než začnete používat ovládací prvek extenderů na stránce, musíte přidat ovládací prvek ScriptManager na stránku.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="e5ebe-138">Následujícím postupem použít zařízení extender ConfirmButton ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="e5ebe-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="e5ebe-139">Vytvořit novou stránku ASP.NET s názvem ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="e5ebe-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="e5ebe-140">Přidání ovládacího prvku ScriptManager na stránce přetažením ovládací prvek na stránce from beneath na kartě Rozšíření AJAX.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="e5ebe-141">Přidejte ovládací prvek standardní tlačítko na stránce přetažením tlačítko from beneath standardní kartu na panelu nástrojů na plochu návrháře.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="e5ebe-142">Klikněte na tlačítko **přidat zařízení Extender** úloh možnost (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="e5ebe-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="e5ebe-143">V dialogovém okně Zvolit rozšíření vyberte ConfirmButtonExtender (viz obrázek 5) a klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="e5ebe-144">Vyberte ovládací prvek tlačítko v návrháři a rozbalte zařízení Extender Button1\_ConfirmButtonExtender uzel v okně Vlastnosti (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="e5ebe-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="e5ebe-145">Přiřaďte hodnotu *skutečně?* ConfirmText vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="e5ebe-146">Spuštění stránky tak, že vyberete možnost nabídky **ladit, spustit ladění** nebo stiskněte klávesu F5.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="e5ebe-147">[![Možnost přidat rozšíření úloh](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="e5ebe-148">**Obrázek 04**: možnost úloh pro přidání zařízení Extender ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="e5ebe-149">[![Výběr zařízení extender ConfirmButton ovládacího prvku](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="e5ebe-150">**Obrázek 05**: výběr – extender ConfirmButton ovládacího prvku ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="e5ebe-151">[![Nastavení vlastnosti ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="e5ebe-152">**Obrázek 06**: nastavení vlastnosti ConfirmButton ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="e5ebe-153">Po otevření stránky, měli byste vidět tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="e5ebe-154">Až kliknete na tlačítko, zobrazí potvrzovací dialogové okno na obrázku 7.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="e5ebe-155">[![Zobrazení dialogového okna potvrzení](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="e5ebe-156">**Obrázek 07**: zobrazení dialogového okna potvrzení ([kliknutím ji zobrazíte obrázek v plné velikosti](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="e5ebe-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="e5ebe-157">Všimněte si, že je obvykle není přetáhnout – extender ovládacího prvku na stránku.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="e5ebe-158">Místo toho použít **přidat zařízení Extender** úlohy přidat rozšiřující ovládací prvek, který jste už přidali na stránku.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="e5ebe-159">Všimněte si kromě toho, že nastavíte ovládacího prvku vlastnosti rozšíření tak, že otevřete okno vlastností pro ovládací prvek se rozšiřuje.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="e5ebe-160">Jeden ovládací prvek ASP.NET, je možné rozšířit pomocí více zařízení Extender ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="e5ebe-161">Seznam vlastností pro ovládací prvek se rozšiřuje se zobrazí všechny ovládací prvek extenderů přidružený k ovládacímu prvku.</span><span class="sxs-lookup"><span data-stu-id="e5ebe-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e5ebe-162">[Předchozí](get-started-with-the-ajax-control-toolkit-vb.md)
> [další](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e5ebe-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
