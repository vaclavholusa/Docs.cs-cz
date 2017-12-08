---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: "Ověřování pomocí datové poznámky validátory (VB) | Microsoft Docs"
author: microsoft
description: "Výhodou vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Naučte se používat s různými typy validátoru..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 227c1acb5e478047c4e5cdc7dbddedd703e91292
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="cf9a1-104">Ověřování pomocí datové poznámky validátory (VB)</span><span class="sxs-lookup"><span data-stu-id="cf9a1-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="cf9a1-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cf9a1-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cf9a1-106">Výhodou vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="cf9a1-107">Naučte se používat s různými typy validátoru atributy a pracovat s nimi v Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="cf9a1-108">V tomto kurzu zjistěte, jak používat validátory datové poznámky k provedení ověření v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="cf9a1-109">Výhodou použití datové poznámky validátory je umožňují vám provést ověření, jednoduše přidáním jeden nebo více atributů – například požadované nebo StringLength atributu – vlastnost třídy.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="cf9a1-110">Než budete moct použít validátory datové poznámky, je nutné stáhnout vazač modelu dat poznámky.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="cf9a1-111">Vazač vzorek dat poznámky modelu můžete stáhnout z webu CodePlex kliknutím [zde](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="cf9a1-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="cf9a1-112">Je důležité si uvědomit, že vazač modelu dat poznámky není oficiální součástí Microsoft ASP.NET MVC framework.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="cf9a1-113">I když vazač modelu dat poznámky vytvořený týmem Microsoft ASP.NET MVC, Microsoft nenabízí oficiální produktovou podporu pro vazač modelu dat poznámky popsané a použili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="cf9a1-114">Pomocí vazače modelu dat poznámky</span><span class="sxs-lookup"><span data-stu-id="cf9a1-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="cf9a1-115">Chcete-li použít vazač modelu anotací dat v aplikaci ASP.NET MVC, musíte nejprve přidat odkaz na sestavení Microsoft.Web.Mvc.DataAnnotations.dll a System.ComponentModel.DataAnnotations.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="cf9a1-116">Vyberte možnost nabídky **projektu, přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="cf9a1-117">Pak klikněte na **Procházet** kartě a přejděte do umístění, které jste stáhli (a rozbalené) ukázková Data vazač modelu poznámky (v tématu **obrázek 1**).</span><span class="sxs-lookup"><span data-stu-id="cf9a1-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="cf9a1-118">**Obrázek 1**: Přidání odkazu na vazač modelu dat poznámky ([Kliknutím zobrazit obrázek v plné velikosti](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="cf9a1-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="cf9a1-119">Vyberte sestavu Microsoft.Web.Mvc.DataAnnotations.dll i System.ComponentModel.DataAnnotations.dll sestavení a klikněte na **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="cf9a1-120">Nelze použít System.ComponentModel.DataAnnotations.dll sestavení součástí rozhraní .NET Framework Service Pack 1 vazač modelu dat poznámky.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="cf9a1-121">Musíte použít verzi sestavení System.ComponentModel.DataAnnotations.dll součástí ke stažení ukázky vazač modelu dat poznámky.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="cf9a1-122">Nakonec budete muset registraci vazač modelu DataAnnotations v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="cf9a1-123">Přidejte následující řádek kódu do aplikace\_Start() obslužné rutiny události tak, aby aplikace\_metodu Start() vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cf9a1-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="cf9a1-124">Tento řádek kódu zaregistruje DataAnnotationsModelBinder jako výchozí vazač modelu pro celou aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="cf9a1-125">Pomocí atributů ověření dat poznámky</span><span class="sxs-lookup"><span data-stu-id="cf9a1-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="cf9a1-126">Pokud používáte vazač modelu dat poznámky, použijete validátoru atributy provést ověření.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="cf9a1-127">Obor názvů System.ComponentModel.DataAnnotations zahrnuje následující atributy ověření:</span><span class="sxs-lookup"><span data-stu-id="cf9a1-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="cf9a1-128">V rozsahu – umožňuje ověřit, zda hodnota vlastnosti leží mezi zadaný rozsah hodnot.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="cf9a1-129">ReqularExpression – umožňuje ověřit, zda hodnota vlastnosti odpovídá zadanému regulárnímu výrazu vzoru.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="cf9a1-130">Požadované – umožňuje označit vlastnost jako povinnou.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="cf9a1-131">StringLength – umožňuje určit maximální délku ve vlastnosti string.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="cf9a1-132">Ověření – základní třída pro všechny atributy validátoru.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="cf9a1-133">Pokud vaše potřeby ověření nejsou splňuje jakákoliv standardní validátory vždy máte možnost vytvořit vlastní validátor atribut dědění nový atribut validátoru ze základní ověřovací atribut.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="cf9a1-134">Třída produktu v **výpis 1** ukazuje, jak se používají tyto atributy validátoru.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="cf9a1-135">Název, popis a UnitPrice vlastnosti jsou označené podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="cf9a1-136">Vlastnost název musí mít délku řetězce, která je menší než 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="cf9a1-137">Vlastnost UnitPrice musí odpovídat nakonec vzor regulárního výrazu, který představuje částku měny.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="cf9a1-138">**Výpis 1**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="cf9a1-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="cf9a1-139">Třída produktu ukazuje, jak používat jeden další atribut: atribut DisplayName.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="cf9a1-140">Atribut DisplayName umožňuje upravit název vlastnosti, když je vlastnost zobrazena v chybové zprávě.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="cf9a1-141">Místo zobrazení chybová zpráva "je požadované pole UnitPrice" můžete zobrazit chybová zpráva "cena pole je požadováno".</span><span class="sxs-lookup"><span data-stu-id="cf9a1-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="cf9a1-142">Pokud chcete úplně přizpůsobit chybové zprávy zobrazí validátor můžete přiřadit vlastní chybová zpráva pro vlastnost ErrorMessage validátoru takto:`<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="cf9a1-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="cf9a1-143">Můžete použít třídu produktu v **výpis 1** s akce kontroleru Create() v **výpis 2**.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="cf9a1-144">Tato akce kontroleru znovu vytvořit zobrazení zobrazí, pokud stav modelu, který obsahuje všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="cf9a1-145">**Výpis 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="cf9a1-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="cf9a1-146">Nakonec můžete vytvořit zobrazení v **výpis 3** akce Create() kliknete pravým tlačítkem a výběrem možnosti nabídky **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="cf9a1-147">Vytvoření zobrazení silného typu pomocí třídy produktu jako třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="cf9a1-148">Vyberte **vytvořit** z zobrazení obsahu rozevíracího seznamu (viz **na obrázku 2**).</span><span class="sxs-lookup"><span data-stu-id="cf9a1-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="cf9a1-149">**Obrázek 2**: Přidání zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="cf9a1-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="cf9a1-150">**Výpis 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="cf9a1-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="cf9a1-151">Odebrat z formuláře vytvořit generované pole Id **přidat zobrazení** možnost nabídky.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="cf9a1-152">Protože v poli Id odpovídá sloupec Identity, nechcete povolit uživatelům zadat hodnotu pro toto pole.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="cf9a1-153">Pokud odesláním formuláře pro vytváření produktu a nezadáte hodnoty pro povinná pole, pak chybu ověření zprávy v **obrázek 3** jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="cf9a1-154">**Obrázek 3**: Chybí povinná pole</span><span class="sxs-lookup"><span data-stu-id="cf9a1-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="cf9a1-155">Pokud zadáte neplatný měna dobu a potom chybovou zprávu ve **obrázek 4** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="cf9a1-156">**Obrázek 4**: Neplatná měna velikost</span><span class="sxs-lookup"><span data-stu-id="cf9a1-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="cf9a1-157">Validátory poznámky Data pomocí rozhraní Entity Framework</span><span class="sxs-lookup"><span data-stu-id="cf9a1-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="cf9a1-158">Pokud používáte Microsoft Entity Framework pro generování tříd modelu dat nelze použít atributy validátoru přímo do vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="cf9a1-159">Protože Entity Framework Designer generuje třídy modelu, všechny změny provedené u třídy modelu budou přepsány při příštím provedení změn v návrháři.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="cf9a1-160">Pokud chcete použít validátory s třídami generované rozhraní Entity Framework budete muset vytvořit datové třídy metadat.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="cf9a1-161">Validátory můžete použít k třídě meta data místo použití validátory ke skutečné třídě.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="cf9a1-162">Představte si například, že jste vytvořili film třídy pomocí rozhraní Entity Framework (viz **obrázek 5**).</span><span class="sxs-lookup"><span data-stu-id="cf9a1-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="cf9a1-163">Představte si kromě toho, že chcete, aby název filmu a ředitel vlastnosti požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="cf9a1-164">V takovém případě můžete vytvořit částečné třídy a třídy dat meta v **výpis 4**.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="cf9a1-165">**Obrázek 5**: Třída film generované Entity Framework</span><span class="sxs-lookup"><span data-stu-id="cf9a1-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="cf9a1-166">**Výpis 4**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="cf9a1-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="cf9a1-167">Soubor v **výpis 4** obsahuje dvě třídy s názvem film a MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="cf9a1-168">Třída film je konkrétní třídu.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-168">The Movie class is a partial class.</span></span> <span data-ttu-id="cf9a1-169">Odpovídá třídu generované Entity Frameworku, který je obsažen v souboru DataModel.Designer.vb.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="cf9a1-170">Rozhraní .NET framework v současné době nepodporuje částečné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="cf9a1-171">Proto neexistuje žádný způsob, jak použít atributy ověření pro vlastnosti film třídy definované v souboru DataModel.Designer.vb aplikací atributů ověření vlastnosti třídy filmu, které jsou definované v souboru v **výpis 4**.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="cf9a1-172">Všimněte si, že třídu film opatřen atributem MetadataType, která odkazuje na třídu MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="cf9a1-173">Třída MovieMetaData obsahuje vlastnosti proxy pro vlastnosti třídy film.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="cf9a1-174">Validátor atributy jsou použity k vlastnostem třídy rozhraní MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="cf9a1-175">Název, ředitele a DateReleased vlastnosti jsou označeny jako požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="cf9a1-176">Vlastnost ředitel musí být přiřazen řetězec, který obsahuje méně než 5 znaků.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="cf9a1-177">Nakonec je použit atribut DisplayName vlastnost DateReleased zobrazíte chybovou zprávu jako "datum vydání pole je povinné."</span><span class="sxs-lookup"><span data-stu-id="cf9a1-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="cf9a1-178">místo chyba "DateReleased pole je požadováno."</span><span class="sxs-lookup"><span data-stu-id="cf9a1-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="cf9a1-179">Všimněte si, že proxy vlastnosti ve třídě MovieMetaData, není nutné představují stejné typy jako odpovídající vlastnosti ve třídě film.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="cf9a1-180">Vlastnost ředitel je například řetězcová vlastnost ve třídě film a vlastnosti objektu ve třídě MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="cf9a1-181">Stránka v **obrázek 6** znázorňuje chybové zprávy vracené při zadání neplatné hodnoty pro vlastnosti film.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="cf9a1-182">**Obrázek 6**: validátory pomocí rozhraní Entity Framework ([Kliknutím zobrazit obrázek v plné velikosti](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="cf9a1-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="cf9a1-183">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cf9a1-183">Summary</span></span>

<span data-ttu-id="cf9a1-184">V tomto kurzu jste zjistili, jak chcete využít výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="cf9a1-185">Jste zjistili, jak používat různé typy ověření jako je povinná a StringLength atributů.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="cf9a1-186">Také jste zjistili, jak používat tyto atributy při práci se službou Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cf9a1-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="cf9a1-187">Předchozí</span><span class="sxs-lookup"><span data-stu-id="cf9a1-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
