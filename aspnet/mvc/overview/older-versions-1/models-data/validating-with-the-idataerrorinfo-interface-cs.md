---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Ověřování s idataerrorinfo – rozhraní (C#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther ukazuje způsob zobrazení chybové zprávy ověření na vlastní implementací rozhraní idataerrorinfo – ve třídu modelu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="9426c-103">Ověřování s idataerrorinfo – rozhraní (C#)</span><span class="sxs-lookup"><span data-stu-id="9426c-103">Validating with the IDataErrorInfo Interface (C#)</span></span>
====================
<span data-ttu-id="9426c-104">podle [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="9426c-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="9426c-105">Stephen Walther ukazuje způsob zobrazení chybové zprávy ověření na vlastní implementací rozhraní idataerrorinfo – ve třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="9426c-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="9426c-106">Cílem tohoto kurzu je vysvětlit, jeden z přístupů k provádění ověření v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9426c-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="9426c-107">Zjistíte, jak zabránit odeslání formuláře HTML bez zadání hodnoty povinných polí formuláře.</span><span class="sxs-lookup"><span data-stu-id="9426c-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="9426c-108">V tomto kurzu zjistěte, jak provádět ověření pomocí rozhraní IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="9426c-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="9426c-109">Předpoklady</span><span class="sxs-lookup"><span data-stu-id="9426c-109">Assumptions</span></span>

<span data-ttu-id="9426c-110">V tomto kurzu budete používám MoviesDB databáze a tabulky databáze filmy.</span><span class="sxs-lookup"><span data-stu-id="9426c-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="9426c-111">Tato tabulka obsahuje následující sloupce:</span><span class="sxs-lookup"><span data-stu-id="9426c-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| <span data-ttu-id="9426c-112">**Název sloupce**</span><span class="sxs-lookup"><span data-stu-id="9426c-112">**Column Name**</span></span> | <span data-ttu-id="9426c-113">**Datový typ**</span><span class="sxs-lookup"><span data-stu-id="9426c-113">**Data Type**</span></span> | <span data-ttu-id="9426c-114">**Povolit hodnoty Null**</span><span class="sxs-lookup"><span data-stu-id="9426c-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="9426c-115">ID</span><span class="sxs-lookup"><span data-stu-id="9426c-115">Id</span></span> | <span data-ttu-id="9426c-116">celá čísla</span><span class="sxs-lookup"><span data-stu-id="9426c-116">Int</span></span> | <span data-ttu-id="9426c-117">False</span><span class="sxs-lookup"><span data-stu-id="9426c-117">False</span></span> |
| <span data-ttu-id="9426c-118">Název</span><span class="sxs-lookup"><span data-stu-id="9426c-118">Title</span></span> | <span data-ttu-id="9426c-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="9426c-119">Nvarchar(100)</span></span> | <span data-ttu-id="9426c-120">False</span><span class="sxs-lookup"><span data-stu-id="9426c-120">False</span></span> |
| <span data-ttu-id="9426c-121">Adresář nacházející</span><span class="sxs-lookup"><span data-stu-id="9426c-121">Director</span></span> | <span data-ttu-id="9426c-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="9426c-122">Nvarchar(100)</span></span> | <span data-ttu-id="9426c-123">False</span><span class="sxs-lookup"><span data-stu-id="9426c-123">False</span></span> |
| <span data-ttu-id="9426c-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="9426c-124">DateReleased</span></span> | <span data-ttu-id="9426c-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="9426c-125">DateTime</span></span> | <span data-ttu-id="9426c-126">False</span><span class="sxs-lookup"><span data-stu-id="9426c-126">False</span></span> |


<span data-ttu-id="9426c-127">V tomto kurzu používám vygenerovat Moje databáze třídy modelu Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9426c-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="9426c-128">Třída film generované rozhraní Entity Framework se zobrazí na obrázku 1.</span><span class="sxs-lookup"><span data-stu-id="9426c-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="9426c-129">[![Film entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9426c-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="9426c-130">**Obrázek 01**: film entity ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="9426c-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="9426c-131">Další informace o používání rozhraní Entity Framework pro generování tříd modelu databáze, získáte v že mé kurzu názvem vytváření třídy modelu s použitím Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9426c-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="9426c-132">Třída Kontroleru</span><span class="sxs-lookup"><span data-stu-id="9426c-132">The Controller Class</span></span>

<span data-ttu-id="9426c-133">Jsme použil řadič domovské na seznamu filmy a vytvořit nové filmy.</span><span class="sxs-lookup"><span data-stu-id="9426c-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="9426c-134">Kód pro tuto třídu je obsažený v výpis 1.</span><span class="sxs-lookup"><span data-stu-id="9426c-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="9426c-135">**Výpis 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="9426c-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="9426c-136">Třída kontroleru domovské v výpis 1 obsahuje dvě Create() akce.</span><span class="sxs-lookup"><span data-stu-id="9426c-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="9426c-137">Je první akcí zobrazí formulář HTML pro vytvoření nové film.</span><span class="sxs-lookup"><span data-stu-id="9426c-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="9426c-138">Druhou akci Create() provede skutečné vložení nové filmu do databáze.</span><span class="sxs-lookup"><span data-stu-id="9426c-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="9426c-139">Druhou akci Create() je volána, když se zobrazí první Create() akce formuláře je odeslána na server.</span><span class="sxs-lookup"><span data-stu-id="9426c-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="9426c-140">Všimněte si, že druhou akci Create() obsahuje následující řádky kódu:</span><span class="sxs-lookup"><span data-stu-id="9426c-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="9426c-141">IsValid – vlastnost vrací hodnotu false, když dojde k chybě ověření.</span><span class="sxs-lookup"><span data-stu-id="9426c-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="9426c-142">V takovém případě se zobrazí znovu vytvořit zobrazení, které obsahuje formulář HTML pro vytváření film.</span><span class="sxs-lookup"><span data-stu-id="9426c-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="9426c-143">Vytvoření částečné třídy</span><span class="sxs-lookup"><span data-stu-id="9426c-143">Creating a Partial Class</span></span>

<span data-ttu-id="9426c-144">Třída film je generován rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9426c-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="9426c-145">Zobrazí kód pro třídu film Pokud rozbalte soubor MoviesDBModel.edmx v okně Průzkumníka řešení a otevřete soubor MoviesDBModel.Designer.cs v editoru kódu (viz obrázek 2).</span><span class="sxs-lookup"><span data-stu-id="9426c-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="9426c-146">[![Kód pro entitu filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9426c-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="9426c-147">**Obrázek 02**: kód pro entitu Movie ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="9426c-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="9426c-148">Třída film je konkrétní třídu.</span><span class="sxs-lookup"><span data-stu-id="9426c-148">The Movie class is a partial class.</span></span> <span data-ttu-id="9426c-149">To znamená, že jsme přidejte jinou třídu se stejným názvem rozšířit funkce film třídy.</span><span class="sxs-lookup"><span data-stu-id="9426c-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="9426c-150">Přidáme ověřovací logiku novou třídu.</span><span class="sxs-lookup"><span data-stu-id="9426c-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="9426c-151">Přidání třídy v výpis 2 do složky modelů.</span><span class="sxs-lookup"><span data-stu-id="9426c-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="9426c-152">**Výpis 2 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="9426c-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="9426c-153">Všimněte si, že zahrnuje třídy ve výpisu 2 *částečné* modifikátor.</span><span class="sxs-lookup"><span data-stu-id="9426c-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="9426c-154">Žádné metody nebo vlastnosti, které přidáte do této třídy stát součástí třídy film generované rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9426c-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="9426c-155">Přidání OnChanging a OnChanged částečné metody</span><span class="sxs-lookup"><span data-stu-id="9426c-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="9426c-156">Pokud rozhraní Entity Framework vygeneruje třídu entity, rozhraní Entity Framework přidá částečné metody do třídy automaticky.</span><span class="sxs-lookup"><span data-stu-id="9426c-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="9426c-157">Rozhraní Entity Framework generuje OnChanging a OnChanged částečné metody, které odpovídají každé vlastnosti třídy.</span><span class="sxs-lookup"><span data-stu-id="9426c-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="9426c-158">V případě film třídu Entity Framework vytvoří následující metody:</span><span class="sxs-lookup"><span data-stu-id="9426c-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="9426c-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="9426c-159">OnIdChanging</span></span>
- <span data-ttu-id="9426c-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="9426c-160">OnIdChanged</span></span>
- <span data-ttu-id="9426c-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="9426c-161">OnTitleChanging</span></span>
- <span data-ttu-id="9426c-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="9426c-162">OnTitleChanged</span></span>
- <span data-ttu-id="9426c-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="9426c-163">OnDirectorChanging</span></span>
- <span data-ttu-id="9426c-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="9426c-164">OnDirectorChanged</span></span>
- <span data-ttu-id="9426c-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="9426c-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="9426c-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="9426c-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="9426c-167">OnChanging metoda je volána správné, než je odpovídající vlastnost změnit.</span><span class="sxs-lookup"><span data-stu-id="9426c-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="9426c-168">Po změně vlastnost, je volána metoda OnChanged správné.</span><span class="sxs-lookup"><span data-stu-id="9426c-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="9426c-169">Můžete využít výhod těchto částečné metody třídy film přidat logiku ověření.</span><span class="sxs-lookup"><span data-stu-id="9426c-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="9426c-170">Aktualizace třídy film v výpis 3 ověřuje, že vlastnosti nadpisu a ředitel přiřazené neprázdných hodnot.</span><span class="sxs-lookup"><span data-stu-id="9426c-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9426c-171">Částečné metody je metoda definovaný ve třídě, která nepotřebujete k implementaci.</span><span class="sxs-lookup"><span data-stu-id="9426c-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="9426c-172">Pokud nemáte implementovat částečné metodu kompilátor odebere podpis metody a všechna volání do metody tedy s sebou se neplatí běhu přidružené k částečné metodě.</span><span class="sxs-lookup"><span data-stu-id="9426c-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="9426c-173">V editoru Visual Studio Code, můžete přidat částečné metoda zadáním klíčové slovo *částečné* následované mezerou zobrazíte seznam částečné k implementaci.</span><span class="sxs-lookup"><span data-stu-id="9426c-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="9426c-174">**Výpis 3 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="9426c-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="9426c-175">Například pokud se pokusíte přiřadit prázdný řetězec pro vlastnost název, potom chybovou zprávu je přiřazen do slovníku nazvaného \_chyby.</span><span class="sxs-lookup"><span data-stu-id="9426c-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="9426c-176">V tomto okamžiku nic ve skutečnosti se stane, když přiřadíte prázdný řetězec pro vlastnost název a chyba se přidá privátní \_chyby pole.</span><span class="sxs-lookup"><span data-stu-id="9426c-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="9426c-177">Musíme implementovat rozhraní idataerrorinfo – vystavit tyto chyby ověření do rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9426c-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="9426c-178">Implementace idataerrorinfo – rozhraní</span><span class="sxs-lookup"><span data-stu-id="9426c-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="9426c-179">Idataerrorinfo – rozhraní byl součástí rozhraní .NET framework od první verze.</span><span class="sxs-lookup"><span data-stu-id="9426c-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="9426c-180">Toto rozhraní je velmi jednoduché rozhraní:</span><span class="sxs-lookup"><span data-stu-id="9426c-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="9426c-181">Pokud třída implementuje idataerrorinfo – rozhraní, rozhraní ASP.NET MVC používat toto rozhraní při vytvoření instance třídy.</span><span class="sxs-lookup"><span data-stu-id="9426c-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="9426c-182">Instance třídy film lze například řadičem domovské Create() akce:</span><span class="sxs-lookup"><span data-stu-id="9426c-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="9426c-183">Rozhraní ASP.NET MVC vytvoří instanci film předaný akce Create() pomocí vazač modelu (DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="9426c-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="9426c-184">Vazač modelu zodpovídá za vytvoření instance objektu film pomocí vytvoření vazby pole formuláře HTML na instanci objektu film.</span><span class="sxs-lookup"><span data-stu-id="9426c-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="9426c-185">DefaultModelBinder zjišťuje, zda třída implementuje idataerrorinfo – rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9426c-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="9426c-186">Pokud toto rozhraní implementuje třídu vazač modelu vyvolá IDataErrorInfo.this indexeru pro každou vlastnost třídy.</span><span class="sxs-lookup"><span data-stu-id="9426c-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="9426c-187">Pokud indexeru vrátí chybovou zprávu přidá vazač modelu tato chybová zpráva pro modelování stavu automaticky.</span><span class="sxs-lookup"><span data-stu-id="9426c-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="9426c-188">DefaultModelBinder také zkontroluje vlastnost IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="9426c-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="9426c-189">Tato vlastnost slouží k reprezentaci konkrétní ověření jiných vlastností chyby související s třídou.</span><span class="sxs-lookup"><span data-stu-id="9426c-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="9426c-190">Například můžete chtít vynutit ověřovací pravidlo, které závisí na hodnotách více vlastností třídy film.</span><span class="sxs-lookup"><span data-stu-id="9426c-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="9426c-191">V takovém případě by vrátit chybu ověření z vlastnosti chyby.</span><span class="sxs-lookup"><span data-stu-id="9426c-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="9426c-192">Aktualizované film třídy ve výpisu 4 implementuje idataerrorinfo – rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9426c-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="9426c-193">**Výpis 4 - Models\Movie.cs (implementuje idataerrorinfo –)**</span><span class="sxs-lookup"><span data-stu-id="9426c-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="9426c-194">Výpis 4 kontroluje vlastnost indexeru \_kolekce chyb, které chcete zobrazit, pokud obsahuje klíč, který odpovídá názvu vlastnosti předaný indexeru.</span><span class="sxs-lookup"><span data-stu-id="9426c-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="9426c-195">Pokud se nezobrazí žádná chyba ověření přidružená vlastnost je vrácen prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="9426c-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="9426c-196">Není nutné upravit řadičem domovské žádným způsobem použití upravené třídy film.</span><span class="sxs-lookup"><span data-stu-id="9426c-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="9426c-197">Stránka zobrazená na obrázku 3 znázorňuje, co se stane, když je pro pole formuláře název nebo ředitel zadána žádná hodnota.</span><span class="sxs-lookup"><span data-stu-id="9426c-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="9426c-198">[![Automatické vytváření metody akce](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9426c-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="9426c-199">**Obrázek 03**: formulář s chybějící hodnoty ([Kliknutím zobrazit obrázek v plné velikosti](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9426c-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="9426c-200">Všimněte si, že automaticky ověření DateReleased hodnoty.</span><span class="sxs-lookup"><span data-stu-id="9426c-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="9426c-201">Vzhledem k tomu, že vlastnost DateReleased nepřijímá hodnoty NULL, DefaultModelBinder generuje automaticky chybu ověření pro tuto vlastnost tak, pokud nemá hodnotu uvedené.</span><span class="sxs-lookup"><span data-stu-id="9426c-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="9426c-202">Pokud chcete upravit chybovou zprávu pro vlastnost DateReleased budete muset vytvořit vlastní vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="9426c-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="9426c-203">Souhrn</span><span class="sxs-lookup"><span data-stu-id="9426c-203">Summary</span></span>

<span data-ttu-id="9426c-204">V tomto kurzu jste se naučili idataerrorinfo – rozhraní použít ke generování chybových zpráv ověření.</span><span class="sxs-lookup"><span data-stu-id="9426c-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="9426c-205">Nejdřív jsme vytvořili částečné film třídu, která rozšiřuje funkce film třídu generované rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9426c-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="9426c-206">V dalším kroku jsme přidali logiku ověření film třída OnTitleChanging() a OnDirectorChanging() částečné metody.</span><span class="sxs-lookup"><span data-stu-id="9426c-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="9426c-207">Nakonec implementovali jsme idataerrorinfo – rozhraní tak, aby získal tyto zprávy ověření na rozhraní ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9426c-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9426c-208">[Předchozí](performing-simple-validation-cs.md)
> [další](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9426c-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
