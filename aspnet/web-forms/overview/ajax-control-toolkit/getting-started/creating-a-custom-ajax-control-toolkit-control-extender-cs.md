---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Vytvoření vlastní AJAX – Extender ovládacího prvku Toolkit (C#) ovládacího prvku | Dokumentace Microsoftu
author: microsoft
description: Vlastní zařízení Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků ASP.NET, bez nutnosti vytvářet nové třídy.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390950"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a><span data-ttu-id="397b1-103">Vytvoření extenderu vlastní AJAX Control Toolkit ovládacího prvku (C#)</span><span class="sxs-lookup"><span data-stu-id="397b1-103">Creating a Custom AJAX Control Toolkit Control Extender (C#)</span></span>
====================
<span data-ttu-id="397b1-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="397b1-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="397b1-105">Vlastní zařízení Extender umožňují přizpůsobit a rozšířit možnosti ovládacích prvků ASP.NET, bez nutnosti vytvářet nové třídy.</span><span class="sxs-lookup"><span data-stu-id="397b1-105">Custom Extenders enable you to customize and extend the capabilities of ASP.NET controls without having to create new classes.</span></span>


<span data-ttu-id="397b1-106">V tomto kurzu se dozvíte, jak vytvořit vlastní – extender ovládacího prvku AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="397b1-106">In this tutorial, you learn how to create a custom AJAX Control Toolkit control extender.</span></span> <span data-ttu-id="397b1-107">Vytvoříme jednoduchý, ale užitečné a nové rozšíření, která změní stav tlačítka ze zakázaného na povolený, když zadáte text do pole TextBox.</span><span class="sxs-lookup"><span data-stu-id="397b1-107">We create a simple, but useful, new extender that changes the state of a Button from disabled to enabled when you type text into a TextBox.</span></span> <span data-ttu-id="397b1-108">Po přečtení tohoto kurzu, budete moct rozšířit ASP.NET AJAX Toolkit s vlastní ovládací prvek extenderů.</span><span class="sxs-lookup"><span data-stu-id="397b1-108">After reading this tutorial, you will be able to extend the ASP.NET AJAX Toolkit with your own control extenders.</span></span>

<span data-ttu-id="397b1-109">Můžete vytvořit vlastní ovládací prvek extenderů pomocí sady Visual Studio nebo Visual Web Developer (ujistěte se, že máte nejnovější verzi aplikace Visual Web Developer).</span><span class="sxs-lookup"><span data-stu-id="397b1-109">You can create custom control extenders using either Visual Studio or Visual Web Developer (make sure that you have the latest version of Visual Web Developer).</span></span>

## <a name="overview-of-the-disabledbutton-extender"></a><span data-ttu-id="397b1-110">Přehled DisabledButton rozšiřujícího objektu</span><span class="sxs-lookup"><span data-stu-id="397b1-110">Overview of the DisabledButton Extender</span></span>

<span data-ttu-id="397b1-111">Naše nové zařízení extender ovládacího prvku má název rozšiřujícího objektu DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="397b1-111">Our new control extender is named the DisabledButton extender.</span></span> <span data-ttu-id="397b1-112">Toto rozšíření bude mít tři vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="397b1-112">This extender will have three properties:</span></span>

- <span data-ttu-id="397b1-113">Vlastnost TargetControlID – textové pole, která rozšiřuje ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="397b1-113">TargetControlID - The TextBox that the control extends.</span></span>
- <span data-ttu-id="397b1-114">TargetButtonIID – tlačítko, které je zakázána nebo povolena.</span><span class="sxs-lookup"><span data-stu-id="397b1-114">TargetButtonIID - The Button that is disabled or enabled.</span></span>
- <span data-ttu-id="397b1-115">DisabledText – text, který je primárně zobrazen na tlačítku.</span><span class="sxs-lookup"><span data-stu-id="397b1-115">DisabledText - The text that is initially displayed in the Button.</span></span> <span data-ttu-id="397b1-116">Když začnete psát, na tomto tlačítku zobrazí hodnoty vlastnosti Text na tlačítku.</span><span class="sxs-lookup"><span data-stu-id="397b1-116">When you start typing, the Button displays the value of the Button Text property.</span></span>

<span data-ttu-id="397b1-117">Můžete integrovat DisabledButton rozšiřujícího objektu do ovládacího prvku textového pole a tlačítko.</span><span class="sxs-lookup"><span data-stu-id="397b1-117">You hook the DisabledButton extender to a TextBox and Button control.</span></span> <span data-ttu-id="397b1-118">Před zadáním textu je tlačítko neaktivní a do textového pole a tlačítko vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="397b1-118">Before you type any text, the Button is disabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

<span data-ttu-id="397b1-119">([Kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-119">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))</span></span>


<span data-ttu-id="397b1-120">Jakmile začnete zadávat text, tlačítko je povoleno a do textového pole a tlačítko vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="397b1-120">After you start typing text, the Button is enabled and the TextBox and Button look like this:</span></span>


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

<span data-ttu-id="397b1-121">([Kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-121">([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))</span></span>


<span data-ttu-id="397b1-122">K vytvoření naší – extender ovládacího prvku, potřebujeme vytvořit následující tři soubory:</span><span class="sxs-lookup"><span data-stu-id="397b1-122">To create our control extender, we need to create the following three files:</span></span>

- <span data-ttu-id="397b1-123">DisabledButtonExtender.cs – tento soubor je třídě ovládacího prvku na straně serveru, který bude spravovat vytváření zařízení extender a umožní nastavit vlastnosti v době návrhu.</span><span class="sxs-lookup"><span data-stu-id="397b1-123">DisabledButtonExtender.cs - This file is the server-side control class that will manage creating your extender and allow you to set the properties at design-time.</span></span> <span data-ttu-id="397b1-124">Definuje také vlastnosti, které lze nastavit pro zařízení extender.</span><span class="sxs-lookup"><span data-stu-id="397b1-124">It also defines the properties that can be set on your extender.</span></span> <span data-ttu-id="397b1-125">Tyto vlastnosti jsou přístupné prostřednictvím kódu a v době návrhu a odpovídá vlastnosti definované v souboru DisableButtonBehavior.js.</span><span class="sxs-lookup"><span data-stu-id="397b1-125">These properties are accessible via code and at design time and match properties defined in the DisableButtonBehavior.js file.</span></span>
- <span data-ttu-id="397b1-126">DisabledButtonBehavior.js--Tento soubor je, ve kterém přidáte všechny logiky skript klienta.</span><span class="sxs-lookup"><span data-stu-id="397b1-126">DisabledButtonBehavior.js -- This file is where you will add all of your client script logic.</span></span>
- <span data-ttu-id="397b1-127">DisabledButtonDesigner.cs – Tato třída umožňuje funkcích při návrhu.</span><span class="sxs-lookup"><span data-stu-id="397b1-127">DisabledButtonDesigner.cs - This class enables design-time functionality.</span></span> <span data-ttu-id="397b1-128">Pokud chcete, aby zařízení extender ovládacího prvku fungovat správně s Visual Studio nebo Visual Web Developer Designer potřebujete této třídy.</span><span class="sxs-lookup"><span data-stu-id="397b1-128">You need this class if you want the control extender to work correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="397b1-129">Extender ovládacího prvku tak se skládá z ovládacího prvku na straně serveru, chování na straně klienta a třídu návrháře na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="397b1-129">So a control extender consists of a server-side control, a client-side behavior, and a server-side designer class.</span></span> <span data-ttu-id="397b1-130">Se dozvíte, jak k vytvoření všech tří z těchto souborů v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="397b1-130">You learn how to create all three of these files in the following sections.</span></span>

## <a name="creating-the-custom-extender-website-and-project"></a><span data-ttu-id="397b1-131">Vytvoření vlastního zařízení Extender webu a projektu</span><span class="sxs-lookup"><span data-stu-id="397b1-131">Creating the Custom Extender Website and Project</span></span>

<span data-ttu-id="397b1-132">Prvním krokem je vytvoření projektu knihovny tříd a webu v aplikaci Visual Studio nebo Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="397b1-132">The first step is to create a class library project and website in Visual Studio/Visual Web Developer.</span></span> <span data-ttu-id="397b1-133">Jsme ll vytvoření vlastního zařízení extender v projektu knihovny tříd a otestování vlastního zařízení extender na webu.</span><span class="sxs-lookup"><span data-stu-id="397b1-133">We�ll create the custom extender in the class library project and test the custom extender in the website.</span></span>

<span data-ttu-id="397b1-134">Umožní začít s webem s.</span><span class="sxs-lookup"><span data-stu-id="397b1-134">Let�s start with the website.</span></span> <span data-ttu-id="397b1-135">Postupujte podle těchto kroků můžete vytvořit na webu:</span><span class="sxs-lookup"><span data-stu-id="397b1-135">Follow these steps to create the website:</span></span>

1. <span data-ttu-id="397b1-136">Vyberte možnost nabídky **soubor, nový web**.</span><span class="sxs-lookup"><span data-stu-id="397b1-136">Select the menu option **File, New Web Site**.</span></span>
2. <span data-ttu-id="397b1-137">Vyberte **Web ASP.NET s** šablony.</span><span class="sxs-lookup"><span data-stu-id="397b1-137">Select the **ASP.NET Web Site** template.</span></span>
3. <span data-ttu-id="397b1-138">Pojmenujte nový web *web1*.</span><span class="sxs-lookup"><span data-stu-id="397b1-138">Name the new website *Website1*.</span></span>
4. <span data-ttu-id="397b1-139">Klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="397b1-139">Click the **OK** button.</span></span>

<span data-ttu-id="397b1-140">V dalším kroku potřeba vytvořit projekt knihovny tříd, který bude obsahovat kód – extender ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="397b1-140">Next, we need to create the class library project that will contain the code for the control extender:</span></span>

1. <span data-ttu-id="397b1-141">Vyberte možnost nabídky **soubor, přidat nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="397b1-141">Select the menu option **File, Add, New Project**.</span></span>
2. <span data-ttu-id="397b1-142">Vyberte **knihovny tříd** šablony.</span><span class="sxs-lookup"><span data-stu-id="397b1-142">Select the **Class Library** template.</span></span>
3. <span data-ttu-id="397b1-143">Pojmenujte novou knihovnu tříd s názvem **CustomExtenders**.</span><span class="sxs-lookup"><span data-stu-id="397b1-143">Name the new class library with the name **CustomExtenders**.</span></span>
4. <span data-ttu-id="397b1-144">Klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="397b1-144">Click the **OK** button.</span></span>

<span data-ttu-id="397b1-145">Po dokončení těchto kroků okna Průzkumník řešení by měl vypadat jako obrázek 1.</span><span class="sxs-lookup"><span data-stu-id="397b1-145">After you complete these steps, your Solution Explorer window should look like Figure 1.</span></span>


<span data-ttu-id="397b1-146">[![Řešení pomocí projektu knihovny webu a třídy](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-146">[![Solution with website and class library project](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)</span></span>

<span data-ttu-id="397b1-147">**Obrázek 01**: řešení pomocí projektu knihovny webu a třídy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-147">**Figure 01**: Solution with website and class library project([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))</span></span>


<span data-ttu-id="397b1-148">Dále je třeba přidat všechny potřebné reference sestavení do projektu knihovny tříd:</span><span class="sxs-lookup"><span data-stu-id="397b1-148">Next, you need to add all of the necessary assembly references to the class library project:</span></span>

1. <span data-ttu-id="397b1-149">Klikněte pravým tlačítkem na projekt CustomExtenders a vyberte možnost nabídky **přidat odkaz**.</span><span class="sxs-lookup"><span data-stu-id="397b1-149">Right-click the CustomExtenders project and select the menu option **Add Reference**.</span></span>
2. <span data-ttu-id="397b1-150">Vyberte kartu .NET.</span><span class="sxs-lookup"><span data-stu-id="397b1-150">Select the .NET tab.</span></span>
3. <span data-ttu-id="397b1-151">Přidejte odkazy na následující sestavení:</span><span class="sxs-lookup"><span data-stu-id="397b1-151">Add references to the following assemblies:</span></span>

    1. <span data-ttu-id="397b1-152">System.Web.dll</span><span class="sxs-lookup"><span data-stu-id="397b1-152">System.Web.dll</span></span>
    2. <span data-ttu-id="397b1-153">System.Web.Extensions.dll</span><span class="sxs-lookup"><span data-stu-id="397b1-153">System.Web.Extensions.dll</span></span>
    3. <span data-ttu-id="397b1-154">System.Design.dll</span><span class="sxs-lookup"><span data-stu-id="397b1-154">System.Design.dll</span></span>
    4. <span data-ttu-id="397b1-155">System.Web.Extensions.Design.dll</span><span class="sxs-lookup"><span data-stu-id="397b1-155">System.Web.Extensions.Design.dll</span></span>
4. <span data-ttu-id="397b1-156">Vyberte kartu Procházet.</span><span class="sxs-lookup"><span data-stu-id="397b1-156">Select the Browse tab.</span></span>
5. <span data-ttu-id="397b1-157">Přidáte odkaz na sestavení AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="397b1-157">Add a reference to the AjaxControlToolkit.dll assembly.</span></span> <span data-ttu-id="397b1-158">Toto sestavení je umístěn ve složce, kam jste stáhli sadou nástrojů AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="397b1-158">This assembly is located in the folder where you downloaded the AJAX Control Toolkit.</span></span>

<span data-ttu-id="397b1-159">Po dokončení těchto kroků, odkazy na složky projektu knihovny tříd by mělo vypadat jako na obrázku 2.</span><span class="sxs-lookup"><span data-stu-id="397b1-159">After you complete these steps, your class library project References folder should look like Figure 2.</span></span>


<span data-ttu-id="397b1-160">[![Složka s odkazy s požadované odkazy](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-160">[![References folder with required references](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)</span></span>

<span data-ttu-id="397b1-161">**Obrázek 02**: složka s odkazy s odkazy na požadovaná ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-161">**Figure 02**: References folder with required references([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))</span></span>


## <a name="creating-the-custom-control-extender"></a><span data-ttu-id="397b1-162">Vytvoření extenderu vlastního ovládacího prvku</span><span class="sxs-lookup"><span data-stu-id="397b1-162">Creating the Custom Control Extender</span></span>

<span data-ttu-id="397b1-163">Teď, když jsme naše knihovny tříd, můžeme začít vytváření naší rozšiřující ovládací prvek.</span><span class="sxs-lookup"><span data-stu-id="397b1-163">Now that we have our class library, we can start building our extender control.</span></span> <span data-ttu-id="397b1-164">Umožní s začínat mnohastránkový třídy ovládacího prvku vlastního zařízení extender (viz seznam 1).</span><span class="sxs-lookup"><span data-stu-id="397b1-164">Let�s start with the bare bones of a custom extender control class (see Listing 1).</span></span>

<span data-ttu-id="397b1-165">**Výpis 1 - MyCustomExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="397b1-165">**Listing 1 - MyCustomExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

<span data-ttu-id="397b1-166">Existuje několik věcí, které zjistíte informace o třídě zařízení extender ovládacího prvku v informacích 1.</span><span class="sxs-lookup"><span data-stu-id="397b1-166">There are several things that you notice about the control extender class in Listing 1.</span></span> <span data-ttu-id="397b1-167">Napřed si všimněte, že třída dědí ze základní třídy ExtenderControlBase.</span><span class="sxs-lookup"><span data-stu-id="397b1-167">First, notice that the class inherits from the base ExtenderControlBase class.</span></span> <span data-ttu-id="397b1-168">Všechny rozšiřujících ovládacích prvků AJAX Control Toolkit odvozovat z této základní třídy.</span><span class="sxs-lookup"><span data-stu-id="397b1-168">All AJAX Control Toolkit extender controls derive from this base class.</span></span> <span data-ttu-id="397b1-169">Základní třída patří třeba Id_cíle vlastnost, která se vyžaduje vlastnost každý – extender ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="397b1-169">For example, the base class includes the TargetID property that is a required property of every control extender.</span></span>

<span data-ttu-id="397b1-170">V dalším kroku Všimněte si, že třída zahrnuje následující dva atributy související s klientského skriptu:</span><span class="sxs-lookup"><span data-stu-id="397b1-170">Next, notice that the class includes the following two attributes related to client script:</span></span>

- <span data-ttu-id="397b1-171">Zobrazení – způsobí, že soubor, který chcete zahrnout jako vložený prostředek v sestavení.</span><span class="sxs-lookup"><span data-stu-id="397b1-171">WebResource - Causes a file to be included as an embedded resource in an assembly.</span></span>
- <span data-ttu-id="397b1-172">ClientScriptResource - způsobí, že prostředek skriptu, který se má načíst ze sestavení.</span><span class="sxs-lookup"><span data-stu-id="397b1-172">ClientScriptResource - Causes a script resource to be retrieved from an assembly.</span></span>

<span data-ttu-id="397b1-173">Atribut webového prostředku se používá pro vložení souboru MyControlBehavior.js JavaScriptu do sestavení při kompilaci vlastního zařízení extender.</span><span class="sxs-lookup"><span data-stu-id="397b1-173">The WebResource attribute is used to embed the MyControlBehavior.js JavaScript file into the assembly when the custom extender is compiled.</span></span> <span data-ttu-id="397b1-174">Atribut ClientScriptResource slouží k načtení skriptu MyControlBehavior.js ze sestavení při použití vlastního zařízení extender na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="397b1-174">The ClientScriptResource attribute is used to retrieve the MyControlBehavior.js script from the assembly when the custom extender is used in a web page.</span></span>


<span data-ttu-id="397b1-175">Aby atributy webového prostředku a ClientScriptResource pracovat musíte zkompilovat soubor jazyka JavaScript jako vložený prostředek.</span><span class="sxs-lookup"><span data-stu-id="397b1-175">In order for the WebResource and ClientScriptResource attributes to work, you must compile the JavaScript file as an embedded resource.</span></span> <span data-ttu-id="397b1-176">Vyberte ho v okně Průzkumníka řešení, otevřete seznam vlastností a přiřaďte hodnotu *integrovaný prostředek* k **akce sestavení** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="397b1-176">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property.</span></span>


<span data-ttu-id="397b1-177">Všimněte si, že – extender ovládacího prvku také obsahuje atribut TargetControlType.</span><span class="sxs-lookup"><span data-stu-id="397b1-177">Notice that the control extender also includes a TargetControlType attribute.</span></span> <span data-ttu-id="397b1-178">Tento atribut slouží k určení typu ovládacího prvku, který je rozšířit pomocí – extender ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="397b1-178">This attribute is used to specify the type of control that is extended by the control extender.</span></span> <span data-ttu-id="397b1-179">V případě výpis 1 extender ovládacího prvku slouží k prodloužení textové pole.</span><span class="sxs-lookup"><span data-stu-id="397b1-179">In the case of Listing 1, the control extender is used to extend a TextBox.</span></span>

<span data-ttu-id="397b1-180">Všimněte si, že vlastního zařízení extender obsahuje vlastnost s názvem MyProperty.</span><span class="sxs-lookup"><span data-stu-id="397b1-180">Finally, notice that the custom extender includes a property named MyProperty.</span></span> <span data-ttu-id="397b1-181">Vlastnost je označena atributem ExtenderControlProperty.</span><span class="sxs-lookup"><span data-stu-id="397b1-181">The property is marked with the ExtenderControlProperty attribute.</span></span> <span data-ttu-id="397b1-182">Metody GetPropertyValue() a SetPropertyValue() slouží k předání hodnotu vlastnosti ze zařízení extender ovládacího prvku na straně serveru k chování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="397b1-182">The GetPropertyValue() and SetPropertyValue() methods are used to pass the property value from the server-side control extender to the client-side behavior.</span></span>

<span data-ttu-id="397b1-183">Umožní s pokračujte a implementujte kód pro naše DisabledButton zařízení extender.</span><span class="sxs-lookup"><span data-stu-id="397b1-183">Let�s go ahead and implement the code for our DisabledButton extender.</span></span> <span data-ttu-id="397b1-184">Kód pro toto rozšíření najdete v informacích 2.</span><span class="sxs-lookup"><span data-stu-id="397b1-184">The code for this extender can be found in Listing 2.</span></span>

<span data-ttu-id="397b1-185">**Výpis 2 - DisabledButtonExtender.cs**</span><span class="sxs-lookup"><span data-stu-id="397b1-185">**Listing 2 - DisabledButtonExtender.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

<span data-ttu-id="397b1-186">Zařízení extender DisabledButton výpis 2 má dvě vlastnosti s názvem TargetButtonID a DisabledText.</span><span class="sxs-lookup"><span data-stu-id="397b1-186">The DisabledButton extender in Listing 2 has two properties named TargetButtonID and DisabledText.</span></span> <span data-ttu-id="397b1-187">IDReferenceProperty použít pro vlastnost TargetButtonID brání přiřazení nic jiného než ID ovládacího prvku tlačítko slouží k této vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="397b1-187">The IDReferenceProperty applied to the TargetButtonID property prevents you from assigning anything other than the ID of a Button control to this property.</span></span>

<span data-ttu-id="397b1-188">Atributy webového prostředku a ClientScriptResource přidružit nachází v souboru s názvem DisabledButtonBehavior.js se toto rozšíření chování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="397b1-188">The WebResource and ClientScriptResource attributes associate a client-side behavior located in a file named DisabledButtonBehavior.js with this extender.</span></span> <span data-ttu-id="397b1-189">Podíváme na tento soubor jazyka JavaScript v další části.</span><span class="sxs-lookup"><span data-stu-id="397b1-189">We discuss this JavaScript file in the next section.</span></span>

## <a name="creating-the-custom-extender-behavior"></a><span data-ttu-id="397b1-190">Vytvoření vlastního zařízení Extender chování</span><span class="sxs-lookup"><span data-stu-id="397b1-190">Creating the Custom Extender Behavior</span></span>

<span data-ttu-id="397b1-191">Komponentu na straně klienta – extender ovládacího prvku se nazývá chování.</span><span class="sxs-lookup"><span data-stu-id="397b1-191">The client-side component of a control extender is called a behavior.</span></span> <span data-ttu-id="397b1-192">Skutečné logiku pro zakázání a povolení tlačítka je součástí DisabledButton chování.</span><span class="sxs-lookup"><span data-stu-id="397b1-192">The actual logic for disabling and enabling the Button is contained in the DisabledButton behavior.</span></span> <span data-ttu-id="397b1-193">Kód jazyka JavaScript pro chování je součástí výpis 3.</span><span class="sxs-lookup"><span data-stu-id="397b1-193">The JavaScript code for the behavior is included in Listing 3.</span></span>

<span data-ttu-id="397b1-194">**Výpis 3 - DisabledButton.js**</span><span class="sxs-lookup"><span data-stu-id="397b1-194">**Listing 3 - DisabledButton.js**</span></span>

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

<span data-ttu-id="397b1-195">Soubor jazyka JavaScript v 3 výpis obsahuje třídu na straně klienta s názvem DisabledButtonBehavior.</span><span class="sxs-lookup"><span data-stu-id="397b1-195">The JavaScript file in Listing 3 contains a client-side class named DisabledButtonBehavior.</span></span> <span data-ttu-id="397b1-196">Tato třída, stejně jako jeho dvojčete na straně serveru zahrnuje dvě vlastnosti s názvem TargetButtonID a získat DisabledText, kterým můžete přistupovat pomocí\_TargetButtonID/set\_TargetButtonID a získejte\_DisabledText/set\_ DisabledText.</span><span class="sxs-lookup"><span data-stu-id="397b1-196">This class, like its server-side twin, includes two properties named TargetButtonID and DisabledText which you can access using get\_TargetButtonID/set\_TargetButtonID and get\_DisabledText/set\_DisabledText.</span></span>

<span data-ttu-id="397b1-197">Metodu initialize() přidruží obslužnou rutinu události keyup cílového elementu chování.</span><span class="sxs-lookup"><span data-stu-id="397b1-197">The initialize() method associates a keyup event handler with the target element for the behavior.</span></span> <span data-ttu-id="397b1-198">Spustí obslužnou rutinu keyup pokaždé, když zadáte nějaké písmeno, do textového pole přidružené k tomuto chování.</span><span class="sxs-lookup"><span data-stu-id="397b1-198">Each time you type a letter into the TextBox associated with this behavior, the keyup handler executes.</span></span> <span data-ttu-id="397b1-199">Keyup obslužná rutina povolí nebo zakáže tlačítko v závislosti na tom, zda textové pole spojených s chováním obsahuje jakýkoli text.</span><span class="sxs-lookup"><span data-stu-id="397b1-199">The keyup handler either enables or disables the Button depending on whether the TextBox associated with the behavior contains any text.</span></span>

<span data-ttu-id="397b1-200">Mějte na paměti, že je nutné kompilovat soubor jazyka JavaScript v informacích 3 jako vložený prostředek.</span><span class="sxs-lookup"><span data-stu-id="397b1-200">Remember that you must compile the JavaScript file in Listing 3 as an embedded resource.</span></span> <span data-ttu-id="397b1-201">Vyberte ho v okně Průzkumníka řešení, otevřete seznam vlastností a přiřaďte hodnotu *integrovaný prostředek* k **akce sestavení** vlastnosti (viz obrázek 3).</span><span class="sxs-lookup"><span data-stu-id="397b1-201">Select the file in the Solution Explorer window, open the property sheet, and assign the value *Embedded Resource* to the **Build Action** property (see Figure 3).</span></span> <span data-ttu-id="397b1-202">Tato možnost je k dispozici v sadě Visual Studio a Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="397b1-202">This option is available in both Visual Studio and Visual Web Developer.</span></span>


<span data-ttu-id="397b1-203">[![Přidání souboru jazyka JavaScript jako vložený prostředek](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-203">[![Adding a JavaScript file as an embedded resource](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)</span></span>

<span data-ttu-id="397b1-204">**Obrázek 03**: přidání souboru jazyka JavaScript jako vložený prostředek ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-204">**Figure 03**: Adding a JavaScript file as an embedded resource([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))</span></span>


## <a name="creating-the-custom-extender-designer"></a><span data-ttu-id="397b1-205">Vytvoření vlastního zařízení Extender návrháře</span><span class="sxs-lookup"><span data-stu-id="397b1-205">Creating the Custom Extender Designer</span></span>

<span data-ttu-id="397b1-206">Existuje jeden poslední třídu, která potřebujeme vytvořit k dokončení našich zařízení extender.</span><span class="sxs-lookup"><span data-stu-id="397b1-206">There is one last class that we need to create to complete our extender.</span></span> <span data-ttu-id="397b1-207">Musíme vytvořit třídu návrháře v informacích 4.</span><span class="sxs-lookup"><span data-stu-id="397b1-207">We need to create the designer class in Listing 4.</span></span> <span data-ttu-id="397b1-208">Tato třída se požadovat, aby zařízení extender s Visual Studio nebo Visual Web Developer Designer se chovají správně.</span><span class="sxs-lookup"><span data-stu-id="397b1-208">This class is required to make the extender behave correctly with the Visual Studio/Visual Web Developer Designer.</span></span>

<span data-ttu-id="397b1-209">**Část 4 – DisabledButtonDesigner.cs**</span><span class="sxs-lookup"><span data-stu-id="397b1-209">**Listing 4 - DisabledButtonDesigner.cs**</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

<span data-ttu-id="397b1-210">Návrhář v informacích 4 přidružíte DisabledButton rozšíření s atributem návrháře. Je třeba použít atribut návrháře pro třídu DisabledButtonExtender takto:</span><span class="sxs-lookup"><span data-stu-id="397b1-210">You associate the designer in Listing 4 with the DisabledButton extender with the Designer attribute.You need to apply the Designer attribute to the DisabledButtonExtender class like this:</span></span>

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a><span data-ttu-id="397b1-211">Pomocí vlastních rozšíření</span><span class="sxs-lookup"><span data-stu-id="397b1-211">Using the Custom Extender</span></span>

<span data-ttu-id="397b1-212">Teď, když jsme dokončili, vytvoření extenderu ovládacího prvku DisabledButton, je čas ho použít v naší webové stránky ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="397b1-212">Now that we have finished creating the DisabledButton control extender, it is time to use it in our ASP.NET website.</span></span> <span data-ttu-id="397b1-213">Nejdřív potřebujeme přidat vlastní rozšíření do panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="397b1-213">First, we need to add the custom extender to the toolbox.</span></span> <span data-ttu-id="397b1-214">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="397b1-214">Follow these steps:</span></span>

1. <span data-ttu-id="397b1-215">Dvojitým kliknutím na stránce v okně Průzkumník řešení otevřete stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="397b1-215">Open an ASP.NET page by double-clicking the page in the Solution Explorer window.</span></span>
2. <span data-ttu-id="397b1-216">Klikněte pravým tlačítkem na panelu nástrojů a vyberte možnost nabídky **zvolit položky**.</span><span class="sxs-lookup"><span data-stu-id="397b1-216">Right-click the toolbox and select the menu option **Choose Items**.</span></span>
3. <span data-ttu-id="397b1-217">V dialogovém okně Zvolit položky panelu nástrojů přejděte na CustomExtenders.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="397b1-217">In the Choose Toolbox Items dialog, browse to the CustomExtenders.dll assembly.</span></span>
4. <span data-ttu-id="397b1-218">Klikněte na tlačítko **OK** tlačítka zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="397b1-218">Click the **OK** button to close the dialog.</span></span>

<span data-ttu-id="397b1-219">Po dokončení těchto kroků – extender ovládacího prvku DisabledButton objevit na panelu nástrojů (viz obrázek 4).</span><span class="sxs-lookup"><span data-stu-id="397b1-219">After you complete these steps, the DisabledButton control extender should appear in the toolbox (see Figure 4).</span></span>


<span data-ttu-id="397b1-220">[![DisabledButton na panelu nástrojů](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-220">[![DisabledButton in the toolbox](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)</span></span>

<span data-ttu-id="397b1-221">**Obrázek 04**: DisabledButton na panelu nástrojů ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-221">**Figure 04**: DisabledButton in the toolbox([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))</span></span>


<span data-ttu-id="397b1-222">Dále musíme vytvořit novou stránku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="397b1-222">Next, we need to create a new ASP.NET page.</span></span> <span data-ttu-id="397b1-223">Postupujte podle těchto kroků:</span><span class="sxs-lookup"><span data-stu-id="397b1-223">Follow these steps:</span></span>

1. <span data-ttu-id="397b1-224">Vytvořte novou stránku ASP.NET s názvem ShowDisabledButton.aspx.</span><span class="sxs-lookup"><span data-stu-id="397b1-224">Create a new ASP.NET page named ShowDisabledButton.aspx.</span></span>
2. <span data-ttu-id="397b1-225">Přetáhněte ovládací prvek ScriptManager na stránce.</span><span class="sxs-lookup"><span data-stu-id="397b1-225">Drag a ScriptManager onto the page.</span></span>
3. <span data-ttu-id="397b1-226">Přetáhněte ovládací prvek textového pole na stránce.</span><span class="sxs-lookup"><span data-stu-id="397b1-226">Drag a TextBox control onto the page.</span></span>
4. <span data-ttu-id="397b1-227">Přetáhněte ovládací prvek tlačítko na stránku.</span><span class="sxs-lookup"><span data-stu-id="397b1-227">Drag a Button control onto the page.</span></span>
5. <span data-ttu-id="397b1-228">V okně Vlastnosti změňte vlastnost ID tlačítek na hodnotu <em>btnSave</em> a vlastnost textu na hodnotu *Uložit\**.</span><span class="sxs-lookup"><span data-stu-id="397b1-228">In the Properties window, change the Button ID property to the value <em>btnSave</em> and the Text property to the value *Save\**.</span></span>
  

<span data-ttu-id="397b1-229">Na stránce jsme vytvořili pomocí standardního ovládacího prvku ASP.NET textového pole a tlačítko.</span><span class="sxs-lookup"><span data-stu-id="397b1-229">We created a page with a standard ASP.NET TextBox and Button control.</span></span>

<span data-ttu-id="397b1-230">Dále musíme rozšířit ovládací prvek TextBox s DisabledButton rozšiřující objekt:</span><span class="sxs-lookup"><span data-stu-id="397b1-230">Next, we need to extend the TextBox control with the DisabledButton extender:</span></span>

1. <span data-ttu-id="397b1-231">Vyberte **přidat zařízení Extender** úloh možnost otevření dialogového okna rozšíření průvodce (viz obrázek 5).</span><span class="sxs-lookup"><span data-stu-id="397b1-231">Select the **Add Extender** task option to open the Extender Wizard dialog (see Figure 5).</span></span> <span data-ttu-id="397b1-232">Všimněte si, že dialogového okna obsahuje naše vlastního zařízení extender DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="397b1-232">Notice that the dialog includes our custom DisabledButton extender.</span></span>
2. <span data-ttu-id="397b1-233">Vyberte zařízení extender DisabledButton a klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="397b1-233">Select the DisabledButton extender and click the **OK** button.</span></span>


<span data-ttu-id="397b1-234">[![Dialogové okno Průvodce rozšiřujícího objektu](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-234">[![The Extender Wizard dialog](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)</span></span>

<span data-ttu-id="397b1-235">**Obrázek 05**: dialogové okno Průvodce zařízení Extender ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-235">**Figure 05**: The Extender Wizard dialog([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))</span></span>


<span data-ttu-id="397b1-236">Nakonec jsme můžete nastavit vlastnosti DisabledButton rozšiřujícího objektu.</span><span class="sxs-lookup"><span data-stu-id="397b1-236">Finally, we can set the properties of the DisabledButton extender.</span></span> <span data-ttu-id="397b1-237">Vlastnosti DisabledButton rozšiřujícího objektu, který můžete upravit tak, že upravíte vlastnosti ovládacího prvku textového pole:</span><span class="sxs-lookup"><span data-stu-id="397b1-237">You can modify the properties of the DisabledButton extender by modifying the properties of the TextBox control:</span></span>

1. <span data-ttu-id="397b1-238">V Návrháři vyberte textového pole.</span><span class="sxs-lookup"><span data-stu-id="397b1-238">Select the TextBox in the Designer.</span></span>
2. <span data-ttu-id="397b1-239">V okně Vlastnosti rozbalte uzel zařízení Extender (viz obrázek 6).</span><span class="sxs-lookup"><span data-stu-id="397b1-239">In the Properties window, expand the Extenders node (see Figure 6).</span></span>
3. <span data-ttu-id="397b1-240">Přiřaďte hodnotu *Uložit* DisabledText vlastnosti a hodnotu *btnSave* TargetButtonID vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="397b1-240">Assign the value *Save* to the DisabledText property and the value *btnSave* to the TargetButtonID property.</span></span>


<span data-ttu-id="397b1-241">[![Nastavení vlastností rozšíření](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-241">[![Setting extender properties](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)</span></span>

<span data-ttu-id="397b1-242">**Obrázek 06**: nastavení vlastností zařízení extender ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-242">**Figure 06**: Setting extender properties([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))</span></span>


<span data-ttu-id="397b1-243">Při spuštění stránky (stisknutím klávesy F5) ovládacího prvku tlačítko je zpočátku zakázáno.</span><span class="sxs-lookup"><span data-stu-id="397b1-243">When you run the page (by hitting F5), the Button control is initially disabled.</span></span> <span data-ttu-id="397b1-244">Jakmile začnete zadávat text do textového pole, tlačítko ovládací prvek je povoleno (viz obrázek 7).</span><span class="sxs-lookup"><span data-stu-id="397b1-244">As soon as you start entering text into the TextBox, the Button control is enabled (see Figure 7).</span></span>


<span data-ttu-id="397b1-245">[![Zařízení extender DisabledButton v akci](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="397b1-245">[![The DisabledButton extender in action](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)</span></span>

<span data-ttu-id="397b1-246">**Obrázek 07**: The DisabledButton rozšiřujícího objektu v akci ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="397b1-246">**Figure 07**: The DisabledButton extender in action([Click to view full-size image](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="397b1-247">Souhrn</span><span class="sxs-lookup"><span data-stu-id="397b1-247">Summary</span></span>

<span data-ttu-id="397b1-248">Cílem tohoto kurzu bylo vysvětlují, jak můžete rozšířit pomocí rozšíření vlastních ovládacích prvků AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="397b1-248">The goal of this tutorial was to explain how you can extend the AJAX Control Toolkit with custom extender controls.</span></span> <span data-ttu-id="397b1-249">V tomto kurzu jsme vytvořili jednoduchý – extender ovládacího prvku DisabledButton.</span><span class="sxs-lookup"><span data-stu-id="397b1-249">In this tutorial, we created a simple DisabledButton control extender.</span></span> <span data-ttu-id="397b1-250">Implementovali jsme toto rozšíření vytvořením třídy DisabledButtonExtender, chování DisabledButtonBehavior JavaScript a DisabledButtonDesigner třídy.</span><span class="sxs-lookup"><span data-stu-id="397b1-250">We implemented this extender by creating a DisabledButtonExtender class, a DisabledButtonBehavior JavaScript behavior, and a DisabledButtonDesigner class.</span></span> <span data-ttu-id="397b1-251">Můžete sledovat podobnou sadu kroků při každém vytvoření rozšíření vlastního ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="397b1-251">You follow a similar set of steps whenever you create a custom control extender.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="397b1-252">[Předchozí](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [další](get-started-with-the-ajax-control-toolkit-vb.md)</span><span class="sxs-lookup"><span data-stu-id="397b1-252">[Previous](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
[Next](get-started-with-the-ajax-control-toolkit-vb.md)</span></span>
