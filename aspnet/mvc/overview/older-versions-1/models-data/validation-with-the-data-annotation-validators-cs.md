---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Ověřování validátory datových poznámek (C#) | Dokumentace Microsoftu
author: microsoft
description: Využijte výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC. Zjistěte, jak použít různé typy ověřování...
ms.author: aspnetcontent
ms.date: 05/29/2009
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 47314f3989b90b1f5d59bda135ea1f9836f2be63
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812811"
---
<a name="validation-with-the-data-annotation-validators-c"></a><span data-ttu-id="e3062-104">Ověřování validátory datových poznámek (C#)</span><span class="sxs-lookup"><span data-stu-id="e3062-104">Validation with the Data Annotation Validators (C#)</span></span>
====================
<span data-ttu-id="e3062-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e3062-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e3062-106">Využijte výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3062-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="e3062-107">Zjistěte, jak použít různé typy atributů ověřovacího modulu a pracovat s nimi v Entity Framework společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e3062-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="e3062-108">V tomto kurzu se dozvíte, jak používat validátory dat. Poznámka k provedení ověření v aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3062-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="e3062-109">Výhodou použití validátory anotace dat je, že umožňují provést ověření, jednoduše tak, že přidáte atribut StringLength nebo jeden nebo více atributů – například požadované – pro vlastnost třídy.</span><span class="sxs-lookup"><span data-stu-id="e3062-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="e3062-110">Před použitím validátory Data poznámky, je nutné stáhnout vazač modelu dat poznámky.</span><span class="sxs-lookup"><span data-stu-id="e3062-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="e3062-111">Ukázky vazače modelu poznámky dat si můžete stáhnout z webu CodePlex kliknutím [tady](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="e3062-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="e3062-112">Je důležité pochopit, že vazač modelu anotací dat není oficiálním součást rozhraní Microsoft ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3062-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="e3062-113">I když vazač modelu dat poznámky vytvořil tým Microsoft ASP.NET MVC, Microsoft nenabízí oficiální technická podpora pro vazač modelu dat poznámky popsané a použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="e3062-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="e3062-114">Použití vazače modelu dat poznámky</span><span class="sxs-lookup"><span data-stu-id="e3062-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="e3062-115">Chcete-li použít vazač modelu anotací dat v aplikaci ASP.NET MVC, musíte nejprve přidat odkaz na sestavení Microsoft.Web.Mvc.DataAnnotations.dll a System.ComponentModel.DataAnnotations.dll sestavení.</span><span class="sxs-lookup"><span data-stu-id="e3062-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="e3062-116">Vyberte možnost nabídky **projektu, přidejte odkaz**.</span><span class="sxs-lookup"><span data-stu-id="e3062-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="e3062-117">Klepnutím na tlačítko **Procházet** kartu a přejděte do umístění, které jste stáhli (a odblokujte) ukázka vazač modelu dat poznámky (naleznete v tématu **obrázek 1**).</span><span class="sxs-lookup"><span data-stu-id="e3062-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

<span data-ttu-id="e3062-118">**Obrázek 1**: Přidání odkazu do vazače modelu dat poznámky ([kliknutím ji zobrazíte obrázek v plné velikosti](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e3062-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image3.png))</span></span>

<span data-ttu-id="e3062-119">Vyberte Microsoft.Web.Mvc.DataAnnotations.dll sestavení a sestavení System.ComponentModel.DataAnnotations.dll a klikněte na tlačítko **OK** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e3062-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="e3062-120">Nelze použít System.ComponentModel.DataAnnotations.dll sestavení součástí .NET Framework Service Pack 1 s vazač modelu dat poznámky.</span><span class="sxs-lookup"><span data-stu-id="e3062-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="e3062-121">Musíte použít verzi sestavení System.ComponentModel.DataAnnotations.dll součástí ke stažení ukázková Data poznámky pro vazač modelu.</span><span class="sxs-lookup"><span data-stu-id="e3062-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="e3062-122">Nakonec budete muset zaregistrovat DataAnnotations vazače modelu v souboru Global.asax.</span><span class="sxs-lookup"><span data-stu-id="e3062-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="e3062-123">Přidejte následující řádek kódu do aplikace\_obslužná rutina události Start() tak, aby aplikace\_metodu Start() vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="e3062-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

<span data-ttu-id="e3062-124">Tento řádek kódu ataAnnotationsModelBinder zaregistruje jako výchozí vazač modelu pro celou aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3062-124">This line of code registers the ataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="e3062-125">Pomocí atributů program pro ověření dat poznámky</span><span class="sxs-lookup"><span data-stu-id="e3062-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="e3062-126">Při použití vazače modelu dat poznámky pomocí atributů ověřovacího modulu provést ověření.</span><span class="sxs-lookup"><span data-stu-id="e3062-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="e3062-127">Obor názvů System.ComponentModel.DataAnnotations zahrnuje následující atributy program pro ověření:</span><span class="sxs-lookup"><span data-stu-id="e3062-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="e3062-128">V rozsahu – umožňuje ověřit, jestli hodnota vlastnosti leží mezi zadaný rozsah hodnot.</span><span class="sxs-lookup"><span data-stu-id="e3062-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="e3062-129">Regulární výraz – umožňuje ověřit, jestli hodnota vlastnosti odpovídá zadanému regulárnímu výrazu vzoru.</span><span class="sxs-lookup"><span data-stu-id="e3062-129">RegularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="e3062-130">Požadováno – umožňuje označit vlastnost jako povinnou.</span><span class="sxs-lookup"><span data-stu-id="e3062-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="e3062-131">StringLength – umožňuje určit maximální délka pro vlastnosti typu string.</span><span class="sxs-lookup"><span data-stu-id="e3062-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="e3062-132">Ověřování – základní třída pro všechny atributy program pro ověření.</span><span class="sxs-lookup"><span data-stu-id="e3062-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e3062-133">Při splnění potřeb ověřování nejsou žádné standardní validátory vždy máte možnost vytvořit vlastní validátor atribut děděním nový atribut ověření z atributu základní ověřování.</span><span class="sxs-lookup"><span data-stu-id="e3062-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="e3062-134">Třída produktu v **výpis 1** ukazuje způsob použití těchto atributů ověřovacího modulu.</span><span class="sxs-lookup"><span data-stu-id="e3062-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="e3062-135">Název, popis a UnitPrice vlastnosti jsou označeny podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e3062-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="e3062-136">Vlastnost Name musí mít délku řetězce, který je kratší než 10 znaků.</span><span class="sxs-lookup"><span data-stu-id="e3062-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="e3062-137">A konečně vlastnost UnitPrice musí odpovídat vzoru regulárního výrazu, který představuje částku měny.</span><span class="sxs-lookup"><span data-stu-id="e3062-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

<span data-ttu-id="e3062-138">**Výpis 1**: Models\Product.cs</span><span class="sxs-lookup"><span data-stu-id="e3062-138">**Listing 1**: Models\Product.cs</span></span>

<span data-ttu-id="e3062-139">Třída produktu ukazuje, jak používat jeden další atribut: atribut DisplayName.</span><span class="sxs-lookup"><span data-stu-id="e3062-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="e3062-140">Atributu DisplayName můžete změnit název vlastnosti, když je vlastnost zobrazena v chybové zprávě.</span><span class="sxs-lookup"><span data-stu-id="e3062-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="e3062-141">Místo zobrazení chybová zpráva "je povinné pole UnitPrice" můžete zobrazit chybová zpráva "Price pole je povinné.".</span><span class="sxs-lookup"><span data-stu-id="e3062-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e3062-142">Pokud chcete kompletně přizpůsobit zprávu o chybě zobrazený validátor můžete přiřadit vlastní chybová zpráva na vlastnost ErrorMessage validátoru takto: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="e3062-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="e3062-143">Můžete použít třídu produktu v **výpis 1** s Create() akce kontroleru v **výpis 2**.</span><span class="sxs-lookup"><span data-stu-id="e3062-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="e3062-144">Tato akce kontroleru znovu zobrazí zobrazení pro vytváření, když ze stavů modelu obsahuje nějaké chyby.</span><span class="sxs-lookup"><span data-stu-id="e3062-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

<span data-ttu-id="e3062-145">**Výpis 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="e3062-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="e3062-146">Nakonec můžete vytvořit zobrazení v **výpis 3** kliknutím pravým tlačítkem na Create() akci a vyberte možnost nabídky **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="e3062-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="e3062-147">Vytvořte zobrazení silného typu pomocí třídy produktu jako třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="e3062-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="e3062-148">Vyberte **vytvořit** z zobrazení obsahu rozevíracího seznamu (viz **obrázek 2**).</span><span class="sxs-lookup"><span data-stu-id="e3062-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

<span data-ttu-id="e3062-149">**Obrázek 2**: Přidání zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="e3062-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

<span data-ttu-id="e3062-150">**Listing 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="e3062-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e3062-151">Id pole odebrat z formuláře vytvořit generovaných **přidat zobrazení** nabídky.</span><span class="sxs-lookup"><span data-stu-id="e3062-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="e3062-152">Protože pole Id odpovídá sloupec Identity, které nechcete povolit uživatelům zadat hodnotu pro toto pole.</span><span class="sxs-lookup"><span data-stu-id="e3062-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="e3062-153">Odeslání formuláře pro vytváření produktu a nezadáte hodnoty pro požadované pole a pak chybu ověření zprávy v **obrázek 3** jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="e3062-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

<span data-ttu-id="e3062-154">**Obrázek 3**: Chybí povinná pole.</span><span class="sxs-lookup"><span data-stu-id="e3062-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="e3062-155">Pokud zadáte neplatný peněžní částce pak chybová zpráva v **obrázek 4** se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="e3062-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

<span data-ttu-id="e3062-156">**Obrázek 4**: Neplatná peněžní částce</span><span class="sxs-lookup"><span data-stu-id="e3062-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="e3062-157">Použití dat anotace validátory sadou Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e3062-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="e3062-158">Pokud používáte Microsoft Entity Framework pro generování tříd modelu dat nelze použít atributy validátor přímo do vaší třídy.</span><span class="sxs-lookup"><span data-stu-id="e3062-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="e3062-159">Protože Návrhář Entity Framework generuje třídy modelu, všechny změny, které provedete tříd modelu budou přepsány při příštím v Návrháři provedete nějaké změny.</span><span class="sxs-lookup"><span data-stu-id="e3062-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="e3062-160">Pokud chcete použít validátory s třídami vygenerovaným rozhraním Entity Framework budete muset vytvořit meta datových tříd.</span><span class="sxs-lookup"><span data-stu-id="e3062-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="e3062-161">Použijete validátory na meta třída dat místo použití validátory ke skutečné třídě.</span><span class="sxs-lookup"><span data-stu-id="e3062-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="e3062-162">Představte si například, že jste vytvořili třídu film používající nástroj Entity Framework (viz **obrázek 5**).</span><span class="sxs-lookup"><span data-stu-id="e3062-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="e3062-163">Představte si kromě toho, že chcete vytvořit název filmu a ředitel pro vlastnosti požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e3062-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="e3062-164">V takovém případě můžete vytvořit částečné třídy a meta třída dat v **výpis 4**.</span><span class="sxs-lookup"><span data-stu-id="e3062-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

<span data-ttu-id="e3062-165">**Obrázek 5**: Třída film vygenerovaným rozhraním Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e3062-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

<span data-ttu-id="e3062-166">**Výpis 4**: Models\Movie.cs</span><span class="sxs-lookup"><span data-stu-id="e3062-166">**Listing 4**: Models\Movie.cs</span></span>

<span data-ttu-id="e3062-167">Soubor v **výpis 4** obsahuje dvě třídy s názvem filmů a MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="e3062-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="e3062-168">Třída Video je částečnou třídu.</span><span class="sxs-lookup"><span data-stu-id="e3062-168">The Movie class is a partial class.</span></span> <span data-ttu-id="e3062-169">Odpovídá vygenerovaným rozhraním Entity Framework, která je součástí souboru DataModel.Designer.vb částečné třídy.</span><span class="sxs-lookup"><span data-stu-id="e3062-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="e3062-170">Rozhraní .NET framework v současné době nepodporuje částečné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e3062-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="e3062-171">Proto neexistuje žádný způsob, jak použití atributů ověření pro vlastnosti film třídy definované v souboru DataModel.Designer.vb použitím atributů ověřovacího modulu na vlastnosti třídy film definované v souboru v **výpis 4**.</span><span class="sxs-lookup"><span data-stu-id="e3062-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="e3062-172">Všimněte si, že video částečné třídy je doplněn o MetadataType atribut, který odkazuje na třídu MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="e3062-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="e3062-173">MovieMetaData třída obsahuje vlastnosti proxy pro vlastnosti třídy Video.</span><span class="sxs-lookup"><span data-stu-id="e3062-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="e3062-174">Program pro ověření atributy jsou použity k vlastnostem třídy MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="e3062-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="e3062-175">Vlastnosti nadpisu, ředitel a DateReleased jsou označeny jako požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e3062-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="e3062-176">Vlastnost ředitel, musíte být přiřazeni řetězec, který obsahuje méně než 5 znaků.</span><span class="sxs-lookup"><span data-stu-id="e3062-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="e3062-177">Nakonec se atribut DisplayName použít pro vlastnost DateReleased zobrazit chybová zpráva jako "datum vydání pole je povinné."</span><span class="sxs-lookup"><span data-stu-id="e3062-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="e3062-178">místo chyby "DateReleased pole je povinné."</span><span class="sxs-lookup"><span data-stu-id="e3062-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="e3062-179">Všimněte si, že proxy vlastnosti ve třídě MovieMetaData, není nutné představují stejné typy jako odpovídající vlastnosti ve třídě video.</span><span class="sxs-lookup"><span data-stu-id="e3062-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="e3062-180">Vlastnost ředitel je například řetězcová vlastnost ve třídě filmů a vlastnosti objektu ve třídě MovieMetaData.</span><span class="sxs-lookup"><span data-stu-id="e3062-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="e3062-181">Na stránce v **obrázek 6** znázorňuje chybové zprávy vracené při zadání neplatné hodnoty pro vlastnosti video.</span><span class="sxs-lookup"><span data-stu-id="e3062-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

<span data-ttu-id="e3062-182">**Obrázek 6**: validátory pomocí Entity Frameworku ([kliknutím ji zobrazíte obrázek v plné velikosti](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="e3062-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-cs/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="e3062-183">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e3062-183">Summary</span></span>

<span data-ttu-id="e3062-184">V tomto kurzu jste zjistili, jak využít výhod vazač modelu dat poznámky k provedení ověření v rámci aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e3062-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="e3062-185">Jste zjistili, jak používat různé typy atributů ověřovacího modulu, jako je například požadovaný a StringLength atributů.</span><span class="sxs-lookup"><span data-stu-id="e3062-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="e3062-186">Také jste zjistili, jak pomocí těchto atributů při práci s rozhraním Entity Framework společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e3062-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e3062-187">[Předchozí](validating-with-a-service-layer-cs.md)
> [další](creating-model-classes-with-the-entity-framework-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e3062-187">[Previous](validating-with-a-service-layer-cs.md)
[Next](creating-model-classes-with-the-entity-framework-vb.md)</span></span>
