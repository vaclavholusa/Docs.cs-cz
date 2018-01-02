---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: "Vytvoření vlastní AJAX ovládacího prvku rozšiřujícího objektu řízení Toolkit (VB) | Microsoft Docs"
author: microsoft
description: "Vlastní Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků technologie ASP.NET, aniž by bylo nutné vytvořit nové třídy."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3e8fceb3c7570aa1bf085c8e1037736254e74ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a><span data-ttu-id="874fe-103">Vytváření vlastních AJAX řízení Toolkit řízení Extender (VB)</span><span class="sxs-lookup"><span data-stu-id="874fe-103">Creating a Custom AJAX Control Toolkit Control Extender (VB)</span></span>
====================
<span data-ttu-id="874fe-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="874fe-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="874fe-105">Vlastní Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků technologie ASP.NET, aniž by bylo nutné vytvořit nové třídy.</span><span class="sxs-lookup"><span data-stu-id="874fe-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="874fe-106">V tomto kurzu zjistěte, jak vytvořit vlastní extender řízení sadu ovládacích prvků AJAX.</span><span class="sxs-lookup"><span data-stu-id="874fe-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="874fe-107">Vytvoříme jednoduchou, ale užitečný, nové rozšiřujícího objektu, který změní stav tlačítko ze zakázaného na povolený, když zadáte text do textové pole.</span><span class="sxs-lookup"><span data-stu-id="874fe-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="874fe-108">Po přečtení tohoto kurzu, bude moct rozšířit s Extender vlastního ovládacího prvku ASP.NET AJAX Toolkit.</span><span class="sxs-lookup"><span data-stu-id="874fe-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="874fe-109">Můžete vytvořit vlastní ovládací prvek Extender pomocí sady Visual Studio nebo Visual Web Developer (ujistěte se, že máte nejnovější verzi aplikace Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="874fe-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="874fe-110">Přehled DisabledButton rozšiřujícího objektu</span><span class="sxs-lookup"><span data-stu-id="874fe-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="874fe-111">Naše nové rozšiřujícího objektu řízení jmenuje DisabledButton rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="874fe-112">Tato rozšiřujícího objektu bude mít tři vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="874fe-112">This extender will have three properties:</span></span>

- <span data-ttu-id="874fe-113">TargetControlID – textové pole, které rozšiřuje ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="874fe-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="874fe-114">TargetButtonIID – tlačítko, které je zakázat nebo povolit.</span><span class="sxs-lookup"><span data-stu-id="874fe-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="874fe-115">DisabledText – text, který je zobrazena v tlačítko.</span><span class="sxs-lookup"><span data-stu-id="874fe-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="874fe-116">Když začnete psát, tlačítko zobrazí hodnotu vlastnosti Text tlačítka.</span><span class="sxs-lookup"><span data-stu-id="874fe-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="874fe-117">Můžete napojit rozšiřujícího objektu DisabledButton do ovládacího prvku textového pole a tlačítko.</span><span class="sxs-lookup"><span data-stu-id="874fe-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="874fe-118">Před zadáním textu je zakázané tlačítko a textové pole a tlačítko vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="874fe-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

<span data-ttu-id="874fe-119">([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))</span></span>


<span data-ttu-id="874fe-120">Až začnete psát text, tlačítko je k dispozici a textové pole a tlačítko vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="874fe-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

<span data-ttu-id="874fe-121">([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="874fe-122">K vytvoření naše rozšiřujícího objektu ovládací prvek, je potřeba vytvořit následující tři soubory:</span><span class="sxs-lookup"><span data-stu-id="874fe-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="874fe-123">DisabledButtonExtender.vb – tento soubor je třída prvek na straně serveru, která bude spravovat vaše rozšiřujícího objektu pro vytváření a vám umožní nastavit vlastnosti v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="874fe-123">DisabledButtonExtender.vb - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="874fe-124">Definuje také vlastnosti, které lze nastavit u vašeho rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="874fe-125">Tyto vlastnosti jsou přístupné prostřednictvím kódu a v době návrhu a odpovídat vlastnosti definované v souboru DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="874fe-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="874fe-126">DisabledButtonBehavior.js – Tento soubor je, kde bude přidejte všechny logika klientský skript.</span><span class="sxs-lookup"><span data-stu-id="874fe-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="874fe-127">DisabledButtonDesigner.vb - Tato třída umožňuje návrh – funkce.</span><span class="sxs-lookup"><span data-stu-id="874fe-127">DisabledButtonDesigner.vb - This class enables design-time functionality.</span></span> <span data-ttu-id="874fe-128">Tato třída je nutné, pokud chcete, aby rozšiřujícího objektu ovládacího prvku pomocí Visual Studio nebo Visual Web Developer návrháře fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="874fe-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="874fe-129">Proto rozšiřujícího objektu ovládacího prvku se skládá z ovládacího prvku na straně serveru, chování klienta a třídu návrháře straně serveru.</span><span class="sxs-lookup"><span data-stu-id="874fe-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="874fe-130">Zjistíte, jak vytvořit všechny tři těchto souborů v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="874fe-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="874fe-131">Vytvoření vlastního webu rozšiřujícího objektu a projektu</span><span class="sxs-lookup"><span data-stu-id="874fe-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="874fe-132">Prvním krokem je vytvoření projektu knihovny tříd a webu v sadě Visual Studio nebo Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="874fe-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="874fe-133">Jsme budete vytvářet vlastní rozšiřujícího objektu v projektu knihovny tříd a testovat vlastní rozšiřujícího objektu na webu.</span><span class="sxs-lookup"><span data-stu-id="874fe-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="874fe-134">Umožní s začínat na webu.</span><span class="sxs-lookup"><span data-stu-id="874fe-134">Let�s start with the website.</span></span> <span data-ttu-id="874fe-135">Postupujte podle těchto kroků můžete vytvořit na webu:</span><span class="sxs-lookup"><span data-stu-id="874fe-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="874fe-136">Vyberte možnost nabídky **soubor, nový web**.</span><span class="sxs-lookup"><span data-stu-id="874fe-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="874fe-137">Vyberte **webové stránky ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="874fe-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="874fe-138">Název nového webu *web1*.</span><span class="sxs-lookup"><span data-stu-id="874fe-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="874fe-139">Klikněte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="874fe-139">Click the **OK** button.</span></span>

<span data-ttu-id="874fe-140">Dále je potřeba vytvořit projektu knihovny tříd, která bude obsahovat kód pro ovládací prvek rozšiřujícího objektu:</span><span class="sxs-lookup"><span data-stu-id="874fe-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="874fe-141">Vyberte možnost nabídky **souboru, přidat nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="874fe-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="874fe-142">Vyberte **knihovny tříd** šablony.</span><span class="sxs-lookup"><span data-stu-id="874fe-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="874fe-143">Název nové třídy knihovny s názvem **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="874fe-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="874fe-144">Klikněte **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="874fe-144">Click the **OK** button.</span></span>

<span data-ttu-id="874fe-145">Po dokončení těchto kroků vaše okna Průzkumníka řešení by mělo vypadat jako obrázek 1.</span><span class="sxs-lookup"><span data-stu-id="874fe-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="874fe-146">[![Řešení s projektem knihovny webu a – třída](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="874fe-147">**Obrázek 01**: řešení s projektem knihovny webu a třídy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))</span></span>


<span data-ttu-id="874fe-148">Dále je nutné přidat všechny potřebné sestavení odkazy na projekt knihovny tříd:</span><span class="sxs-lookup"><span data-stu-id="874fe-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="874fe-149">Klikněte pravým tlačítkem na projekt CustomExtenders a vyberte možnost nabídky **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="874fe-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="874fe-150">Vyberte kartu .NET.</span><span class="sxs-lookup"><span data-stu-id="874fe-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="874fe-151">Přidejte odkazy na následující sestavení:</span><span class="sxs-lookup"><span data-stu-id="874fe-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="874fe-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="874fe-152">System.Web.dll</span></span>
    2. <span data-ttu-id="874fe-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="874fe-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="874fe-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="874fe-154">System.Design.dll</span></span>
    4. <span data-ttu-id="874fe-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="874fe-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="874fe-156">Vyberte kartu Procházet.</span><span class="sxs-lookup"><span data-stu-id="874fe-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="874fe-157">Přidáte odkaz na sestavení AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="874fe-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="874fe-158">Toto sestavení se nachází ve složce, kam jste stáhli Toolkitu AJAX.</span><span class="sxs-lookup"><span data-stu-id="874fe-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="874fe-159">Můžete ověřit, že jste všechny odkazy na pravém přidali pravým tlačítkem myši na projekt, vyberte vlastnosti a kliknutím na kartě odkazy (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="874fe-159">You can verify that you have added all of the right references by right-clicking your project, selecting Properties, and clicking the References tab (see Figure 2).</span></span>


<span data-ttu-id="874fe-160">[![Odkazy na složku se vyžaduje odkazy](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)</span></span>

<span data-ttu-id="874fe-161">**Obrázek 02**: odkazy na složku se vyžaduje odkazy ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="874fe-162">Vytvoření vlastního ovládacího prvku rozšiřujícího objektu</span><span class="sxs-lookup"><span data-stu-id="874fe-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="874fe-163">Teď, když máme naše knihovny tříd, můžeme začít vytváření naše rozšiřující ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="874fe-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="874fe-164">Umožní s začínat mnohastránkový vlastní rozšiřujícího objektu třídy ovládacího prvku (viz seznam 1).</span><span class="sxs-lookup"><span data-stu-id="874fe-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="874fe-165">**Výpis 1 - MyCustomExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="874fe-165">**Listing 1 - MyCustomExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

<span data-ttu-id="874fe-166">Existuje několik věcí, které jste si všimli o třídě ovládacího prvku rozšiřujícího objektu v výpis 1.</span><span class="sxs-lookup"><span data-stu-id="874fe-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="874fe-167">První, Všimněte si, že třídy dědí vlastnosti ze základní třídy ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="874fe-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="874fe-168">Všechny prvky rozšiřujícího objektu AJAX Control Toolkit odvozovat z této základní třídy.</span><span class="sxs-lookup"><span data-stu-id="874fe-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="874fe-169">Například základní třída obsahuje %{targetid/ vlastnost, která se vyžaduje vlastnost každých rozšiřujícího objektu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="874fe-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="874fe-170">V dalším kroku Všimněte si, že třída zahrnuje následující dva atributy související s klientského skriptu:</span><span class="sxs-lookup"><span data-stu-id="874fe-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="874fe-171">Webového prostředku - způsobí, že soubor, který chcete-li být zahrnuta jako vložený prostředek v sestavení.</span><span class="sxs-lookup"><span data-stu-id="874fe-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="874fe-172">ClientScriptResource - způsobí, že prostředek skript má být načtena ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="874fe-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="874fe-173">Atribut webového prostředku slouží k vložení soubor MyControlBehavior.js JavaScript do sestavení při kompilaci vlastní rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="874fe-174">Atribut ClientScriptResource slouží k načtení skriptu MyControlBehavior.js od sestavení, pokud se používá vlastní rozšiřujícího objektu na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="874fe-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="874fe-175">Aby atributy webového prostředku a ClientScriptResource postup musíte zkompilovat soubor JavaScript jako vložený prostředek.</span><span class="sxs-lookup"><span data-stu-id="874fe-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="874fe-176">Vyberte soubor v okně Průzkumníka řešení, otevřete seznam vlastností a přiřadí hodnotu *vložený prostředek* k **akce sestavení** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="874fe-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="874fe-177">Všimněte si, že rozšiřujícího objektu ovládací prvek také obsahuje atribut TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="874fe-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="874fe-178">Tento atribut slouží k určení typu ovládacího prvku rozšířený o rozšiřujícího objektu ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="874fe-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="874fe-179">V případě výpis 1 rozšiřujícího objektu ovládacího prvku slouží k prodloužení textové pole.</span><span class="sxs-lookup"><span data-stu-id="874fe-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="874fe-180">Všimněte si, že vlastní rozšiřujícího objektu obsahuje vlastnost s názvem MyProperty.</span><span class="sxs-lookup"><span data-stu-id="874fe-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="874fe-181">Je vlastnost označena atributem ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="874fe-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="874fe-182">Metody GetPropertyValue() a SetPropertyValue() se používají k předat hodnotu vlastnosti z rozšiřujícího objektu serverové řízení chování klienta.</span><span class="sxs-lookup"><span data-stu-id="874fe-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="874fe-183">Umožní s pokračujte a implementovat kód pro naše DisabledButton rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="874fe-184">Kód pro tento rozšiřujícího objektu naleznete v výpis 2.</span><span class="sxs-lookup"><span data-stu-id="874fe-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="874fe-185">**Výpis 2 - DisabledButtonExtender.vb**</span><span class="sxs-lookup"><span data-stu-id="874fe-185">**Listing 2 - DisabledButtonExtender.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

<span data-ttu-id="874fe-186">Rozšiřujícího objektu DisabledButton v výpis 2 má dvě vlastnosti s názvem TargetButtonID a DisabledText.</span><span class="sxs-lookup"><span data-stu-id="874fe-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="874fe-187">IDReferenceProperty použité na vlastnost TargetButtonID zabrání jakoukoli jinou hodnotu než ID ovládacího prvku tlačítko přiřazení k této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="874fe-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="874fe-188">Atributy webového prostředku a ClientScriptResource přidružit chování klienta nachází v souboru s názvem DisabledButtonBehavior.js se tento rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="874fe-189">Tento soubor JavaScript v další části probereme.</span><span class="sxs-lookup"><span data-stu-id="874fe-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="874fe-190">Vytváření vlastních rozšiřujícího objektu chování</span><span class="sxs-lookup"><span data-stu-id="874fe-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="874fe-191">Komponenta klienta rozšiřujícího objektu ovládacího prvku se nazývá chování.</span><span class="sxs-lookup"><span data-stu-id="874fe-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="874fe-192">Skutečné logiku pro zakázání a povolení tlačítko je obsažený v DisabledButton chování.</span><span class="sxs-lookup"><span data-stu-id="874fe-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="874fe-193">Kód jazyka JavaScript pro chování je součástí výpis 3.</span><span class="sxs-lookup"><span data-stu-id="874fe-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="874fe-194">**Výpis 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="874fe-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

<span data-ttu-id="874fe-195">Soubor JavaScript v výpis 3 obsahuje třídy klienta s názvem DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="874fe-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="874fe-196">Tato třída jako jeho twin serverové zahrnuje dvě vlastnosti s názvem TargetButtonID a získat DisabledText, kterým můžete přistupovat pomocí\_TargetButtonID nebo nastavení\_TargetButtonID a získat\_DisabledText nebo nastavení\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="874fe-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="874fe-197">Metoda inicializaci() přidruží obslužnou rutinu události keyup cílový element chování.</span><span class="sxs-lookup"><span data-stu-id="874fe-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="874fe-198">Pokaždé, když zadáte písmeno do textového pole přidružené k tomuto chování, zpracuje obslužná rutina keyup.</span><span class="sxs-lookup"><span data-stu-id="874fe-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="874fe-199">Obslužná rutina KeyUp – povolí nebo zakáže tlačítko v závislosti na tom, jestli do textového pole přidružené chování obsahuje jakýkoli text.</span><span class="sxs-lookup"><span data-stu-id="874fe-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="874fe-200">Mějte na paměti, že je nutné zkompilovat soubor JavaScript v výpis 3 jako vložený prostředek.</span><span class="sxs-lookup"><span data-stu-id="874fe-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="874fe-201">Vyberte soubor v okně Průzkumníka řešení, otevřete seznam vlastností a přiřadí hodnotu *vložený prostředek* k **akce sestavení** vlastnost (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="874fe-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="874fe-202">Tato možnost je k dispozici v sadě Visual Studio a aplikace Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="874fe-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="874fe-203">[![Přidání souboru jazyka JavaScript jako vloženého prostředku](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)</span></span>

<span data-ttu-id="874fe-204">**Obrázek 03**: přidání souboru jazyka JavaScript jako vložený prostředek ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="874fe-205">Vytváření vlastních rozšiřujícího objektu návrháře</span><span class="sxs-lookup"><span data-stu-id="874fe-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="874fe-206">Neexistuje jeden poslední třída, která potřebujeme k vytvoření k dokončení naše rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="874fe-207">Musíme vytvořit třídu návrháře výpis 4.</span><span class="sxs-lookup"><span data-stu-id="874fe-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="874fe-208">Tato třída je nutná ke správnému rozšiřujícího objektu chovat správně pomocí Visual Studio nebo Visual Web Developer návrháře.</span><span class="sxs-lookup"><span data-stu-id="874fe-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="874fe-209">**Výpis 4 - DisabledButtonDesigner.vb**</span><span class="sxs-lookup"><span data-stu-id="874fe-209">**Listing 4 - DisabledButtonDesigner.vb**</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

<span data-ttu-id="874fe-210">Návrhář v výpis 4 přidružíte rozšiřujícího objektu DisabledButton s atributu Designer. Je třeba použít atributu Designer k třídě DisabledButtonExtender takto:</span><span class="sxs-lookup"><span data-stu-id="874fe-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="874fe-211">Pomocí vlastních rozšiřujícího objektu</span><span class="sxs-lookup"><span data-stu-id="874fe-211">Using the Custom Extender</span></span>

<span data-ttu-id="874fe-212">Teď, když jsme dokončili vytváření rozšiřujícího objektu ovládacího prvku DisabledButton, je čas pro použití v našem webu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="874fe-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="874fe-213">Nejdřív je potřeba přidat vlastní rozšiřujícího objektu do sady nástrojů.</span><span class="sxs-lookup"><span data-stu-id="874fe-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="874fe-214">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="874fe-214">Follow these steps:</span></span>

1. <span data-ttu-id="874fe-215">Otevřete stránku ASP.NET poklepáním na stránku okna Průzkumníka řešení.</span><span class="sxs-lookup"><span data-stu-id="874fe-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="874fe-216">Klikněte pravým tlačítkem na panelu nástrojů a vyberte možnost nabídky **zvolit položky**.</span><span class="sxs-lookup"><span data-stu-id="874fe-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="874fe-217">V dialogovém okně Zvolit položky nástrojů vyhledejte CustomExtenders.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="874fe-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="874fe-218">Klikněte **OK** tlačítko zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="874fe-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="874fe-219">Po dokončení těchto kroků, by se rozšiřujícího objektu ovládacího prvku DisabledButton v panelu nástrojů (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="874fe-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="874fe-220">[![DisabledButton v panelu nástrojů](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)</span></span>

<span data-ttu-id="874fe-221">**Obrázek 04**: DisabledButton v panelu nástrojů ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))</span></span>


<span data-ttu-id="874fe-222">Dále je potřeba vytvořit novou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="874fe-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="874fe-223">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="874fe-223">Follow these steps:</span></span>

1. <span data-ttu-id="874fe-224">Vytvořte novou stránku ASP.NET s názvem ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="874fe-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="874fe-225">Přetáhněte ovládací prvek ScriptManager na stránku.</span><span class="sxs-lookup"><span data-stu-id="874fe-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="874fe-226">Přetáhněte ovládací prvek textové pole na stránce.</span><span class="sxs-lookup"><span data-stu-id="874fe-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="874fe-227">Přetáhněte ovládací prvek tlačítko na stránce.</span><span class="sxs-lookup"><span data-stu-id="874fe-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="874fe-228">V okně vlastností změňte na hodnotu vlastnosti ID tlačítko *btnSave* a vlastnost Text na hodnotu *Uložit\**.</span><span class="sxs-lookup"><span data-stu-id="874fe-228">In the Properties window, change the Button ID property to the value *btnSave* and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="874fe-229">Na stránce jsme vytvořili pomocí standardního ovládacího prvku ASP.NET TextBox a tlačítko.</span><span class="sxs-lookup"><span data-stu-id="874fe-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="874fe-230">Dále je potřeba rozšířit ovládacího prvku textového pole s DisabledButton rozšiřujícího objektu:</span><span class="sxs-lookup"><span data-stu-id="874fe-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="874fe-231">Vyberte **přidat rozšiřujícího objektu** úloh možnost otevřete dialogové okno Průvodce rozšiřujícího objektu (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="874fe-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="874fe-232">Všimněte si, že dialogové okno obsahuje naše vlastní DisabledButton rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="874fe-233">Vyberte rozšiřujícího objektu DisabledButton a klikněte na **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="874fe-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="874fe-234">[![Dialogové okno Průvodce rozšiřujícího objektu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)</span></span>

<span data-ttu-id="874fe-235">**Obrázek 05**: dialogové okno Průvodce rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))</span></span>


<span data-ttu-id="874fe-236">Nakonec jsme můžete nastavit vlastnosti DisabledButton rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="874fe-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="874fe-237">Vlastnosti rozšiřujícího objektu DisabledButton můžete upravit změnou vlastností ovládacího prvku TextBox:</span><span class="sxs-lookup"><span data-stu-id="874fe-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="874fe-238">Vyberte textové pole v návrháři.</span><span class="sxs-lookup"><span data-stu-id="874fe-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="874fe-239">V okně vlastností rozbalte uzel Extender (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="874fe-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="874fe-240">Přiřadí hodnotu *Uložit* vlastnost DisabledText a hodnota *btnSave* TargetButtonID vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="874fe-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="874fe-241">[![Nastavení vlastností rozšiřujícího objektu](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)</span></span>

<span data-ttu-id="874fe-242">**Obrázek 06**: nastavení vlastností rozšiřujícího objektu ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))</span></span>


<span data-ttu-id="874fe-243">Při spuštění stránky (s stále mačkat F5), ovládacího prvku tlačítko původně vypnutá.</span><span class="sxs-lookup"><span data-stu-id="874fe-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="874fe-244">Jakmile začnete zadají text do textového pole, je ovládací prvek tlačítko povoleno (viz obrázek 7).</span><span class="sxs-lookup"><span data-stu-id="874fe-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="874fe-245">[![Rozšiřujícího objektu DisabledButton v akci](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="874fe-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)</span></span>

<span data-ttu-id="874fe-246">**Obrázek 07**: rozšiřujícího objektu DisabledButton v akci ([Kliknutím zobrazit obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="874fe-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="874fe-247">Souhrn</span><span class="sxs-lookup"><span data-stu-id="874fe-247">Summary</span></span>

<span data-ttu-id="874fe-248">Cílem tohoto kurzu bylo vysvětlují, jak můžete rozšířit Toolkitu AJAX s rozšiřující vlastní ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="874fe-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="874fe-249">V tomto kurzu jsme vytvořili jednoduché rozšiřujícího objektu prostředí DisabledButton ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="874fe-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="874fe-250">Implementovali jsme tento rozšiřujícího objektu vytvořením třídy DisabledButtonExtender, chování DisabledButtonBehavior JavaScript a třídu DisabledButtonDesigner.</span><span class="sxs-lookup"><span data-stu-id="874fe-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="874fe-251">Pokaždé, když vytvoříte vlastní ovládací prvek extender provedením podobnou sadu kroků.</span><span class="sxs-lookup"><span data-stu-id="874fe-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="874fe-252">Předchozí</span><span class="sxs-lookup"><span data-stu-id="874fe-252">Previous</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
