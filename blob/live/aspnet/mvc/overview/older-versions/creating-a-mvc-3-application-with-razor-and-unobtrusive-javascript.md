---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: "Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript | Microsoft Docs"
author: microsoft
description: "Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak jednoduché je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor. Aplikace s ukázka..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 68870caf1608e596962650cf653e5b455b82382a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="1f222-104">Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript</span><span class="sxs-lookup"><span data-stu-id="1f222-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="1f222-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1f222-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1f222-106">Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak jednoduché je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="1f222-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="1f222-107">Ukázkovou aplikaci ukazuje, jak používat nový zobrazovací modul Razor s architekturou ASP.NET MVC verze 3 a Visual Studio 2010 k vytvoření fiktivních webu seznam uživatelů, která zahrnuje funkce, jako je vytváření, zobrazení, úpravy a odstraňování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1f222-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="1f222-108">Tento kurz popisuje kroky, které byly provedeny k vytvoření ukázkové aplikace ASP.NET MVC 3 seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1f222-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="1f222-109">Projekt sady Visual Studio se zdrojovým kódem C# a VB je k dispozici v tomto tématu: [Stáhnout](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="1f222-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="1f222-110">Pokud máte dotazy týkající se tohoto kurzu, prosím, aby post [mvcforum](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="1f222-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="1f222-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="1f222-111">Overview</span></span>

<span data-ttu-id="1f222-112">Aplikace, kterou budete sestavení je web seznamu jednoduché uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f222-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="1f222-113">Uživatele můžete zadat, zobrazit a aktualizovat informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="1f222-113">Users can enter, view, and update user information.</span></span>

![Ukázka lokality](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="1f222-115">Stáhnete dokončený projekt jazyka Visual Basic a C# [zde](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="1f222-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="1f222-116">Vytváření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="1f222-116">Creating the Web Application</span></span>

<span data-ttu-id="1f222-117">Pokud chcete spustit tento kurz, otevřete Visual Studio 2010 a vytvořte nový projekt pomocí *webové aplikace ASP.NET MVC 3* šablony.</span><span class="sxs-lookup"><span data-stu-id="1f222-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="1f222-118">Název aplikace &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="1f222-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="1f222-119">[![Nový projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="1f222-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="1f222-120">V **nový ASP.NET MVC 3 projekt** dialogovém okně, vyberte **Internetové aplikace**vyberte zobrazovací modul Razor a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="1f222-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Dialogové okno Nový projekt ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="1f222-122">V tomto kurzu nebudete používat poskytovatele členství prostředí ASP.NET, takže můžete odstranit všechny soubory přidružené k přihlášení a členství.</span><span class="sxs-lookup"><span data-stu-id="1f222-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="1f222-123">V **Průzkumníku**, odeberte tyto soubory a adresáře:</span><span class="sxs-lookup"><span data-stu-id="1f222-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="1f222-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="1f222-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="1f222-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="1f222-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="1f222-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="1f222-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="1f222-127">*Views\Account* (a všechny soubory v tomto adresáři)</span><span class="sxs-lookup"><span data-stu-id="1f222-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="1f222-129">Upravit  *\_Layout.cshtml* souboru a nahraďte kód uvnitř `<div>` element s názvem `logindisplay` se zprávou  *&quot;* zakázáno přihlášení&quot;.</span><span class="sxs-lookup"><span data-stu-id="1f222-129">Edit the *\_Layout.cshtml* file and replace the markup inside the `<div>` element named `logindisplay` with the message *&quot;*Login Disabled&quot;.</span></span> <span data-ttu-id="1f222-130">Následující příklad ukazuje kód nové:</span><span class="sxs-lookup"><span data-stu-id="1f222-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="1f222-131">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="1f222-131">Adding the Model</span></span>

<span data-ttu-id="1f222-132">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a potom klikněte na **– třída**.</span><span class="sxs-lookup"><span data-stu-id="1f222-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nový uživatel Mdl – třída](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="1f222-134">Název třídy `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="1f222-134">Name the class `UserModel`.</span></span> <span data-ttu-id="1f222-135">Nahraďte obsah *UserModel* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1f222-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="1f222-136">`UserModel` Třída reprezentuje uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f222-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="1f222-137">Každý člen třídy je opatřen poznámkou [požadované](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) atribut z [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="1f222-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="1f222-138">Atributy v [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) obor názvů ověřování automatické a server straně klienta pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f222-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="1f222-139">Otevřete `HomeController` třídu a přidejte `using` direktivy, takže může získat přístup `UserModel` a `Users` třídy:</span><span class="sxs-lookup"><span data-stu-id="1f222-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="1f222-140">Právě po `HomeController` deklarace, přidejte následující komentář a odkaz na `Users` třídy:</span><span class="sxs-lookup"><span data-stu-id="1f222-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="1f222-141">`Users` Třída je jednodušší, v paměti datové úložiště, které budete používat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1f222-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="1f222-142">V reálné aplikaci byste použili databázi k uložit informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="1f222-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="1f222-143">Několik prvních řádků `HomeController` souboru jsou uvedeny v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1f222-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="1f222-144">Sestavení aplikace tak, aby uživatelského modelu se bude k dispozici pro Průvodce generováním uživatelského rozhraní v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="1f222-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="1f222-145">Vytváření výchozí zobrazení</span><span class="sxs-lookup"><span data-stu-id="1f222-145">Creating the Default View</span></span>

<span data-ttu-id="1f222-146">Dalším krokem je přidat metodu akce a zobrazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1f222-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="1f222-147">Odstraňte existující *Views\Home\Index* souboru.</span><span class="sxs-lookup"><span data-stu-id="1f222-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="1f222-148">Vytvoří nový *Index* soubor k zobrazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1f222-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="1f222-149">V `HomeController` třídy, nahraďte obsah `Index` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1f222-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="1f222-150">Klepněte pravým tlačítkem myši `Index` metoda a pak klikněte na tlačítko **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1f222-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Přidání zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="1f222-152">Vyberte **vytvořit zobrazení silného typu** možnost.</span><span class="sxs-lookup"><span data-stu-id="1f222-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="1f222-153">Pro **zobrazit třída dat**, vyberte **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="1f222-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="1f222-154">(Pokud nevidíte **Mvc3Razor.Models.UserModel** v **zobrazit třída dat** pole, které potřebujete k vytvoření projektu.) Ujistěte se, že bude zobrazovací modul je nastavená na **Razor**.</span><span class="sxs-lookup"><span data-stu-id="1f222-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="1f222-155">Nastavit **zobrazit obsah** k **seznamu** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1f222-155">Set **View content** to **List** and then click **Add**.</span></span>

![Přidání zobrazení indexu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="1f222-157">Nové zobrazení automaticky scaffolds uživatelská data, která je předána `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f222-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="1f222-158">Zkontrolujte nově vygenerovaný *Views\Home\Index* souboru.</span><span class="sxs-lookup"><span data-stu-id="1f222-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="1f222-159">**Vytvořit nový**, **upravit**, **podrobnosti**, a **odstranit** nefungují odkazy, ale zbývající části stránky je funkční.</span><span class="sxs-lookup"><span data-stu-id="1f222-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="1f222-160">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="1f222-160">Run the page.</span></span> <span data-ttu-id="1f222-161">Zobrazí seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="1f222-161">You see a list of users.</span></span>

![Index stránky](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="1f222-163">Otevřete *Index.cshtml* souboru a nahraďte `ActionLink` kód pro **upravit**, **podrobnosti**, a **odstranit** následujícím kódem :</span><span class="sxs-lookup"><span data-stu-id="1f222-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="1f222-164">Uživatelské jméno se používá jako ID najít vybraný záznam v **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="1f222-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="1f222-165">Vytváření zobrazení podrobností</span><span class="sxs-lookup"><span data-stu-id="1f222-165">Creating the Details View</span></span>

<span data-ttu-id="1f222-166">Dalším krokem je přidání `Details` metody akce a zobrazení, aby bylo možné zobrazit podrobné informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="1f222-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="1f222-168">Přidejte následující `Details` metoda domácí řadiče:</span><span class="sxs-lookup"><span data-stu-id="1f222-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="1f222-169">Klepněte pravým tlačítkem myši `Details` metoda a potom vyberte **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="1f222-169">Right-click inside the `Details` method and then select **Add View**.</span></span> <span data-ttu-id="1f222-170">Ověřte, zda **zobrazit třída dat** obsahuje pole **Mvc3Razor.Models.UserModel***.*</span><span class="sxs-lookup"><span data-stu-id="1f222-170">Verify that the **View data class** box contains **Mvc3Razor.Models.UserModel***.*</span></span> <span data-ttu-id="1f222-171">Nastavit **zobrazit obsah** k **podrobnosti** a pak klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="1f222-171">Set **View content** to **Details** and then click **Add**.</span></span>

![Přidání zobrazení podrobností](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="1f222-173">Spusťte aplikaci a vyberte podrobnosti odkaz.</span><span class="sxs-lookup"><span data-stu-id="1f222-173">Run the application and select a details link.</span></span> <span data-ttu-id="1f222-174">Automatické generování uživatelského rozhraní zobrazuje každou vlastnost v modelu.</span><span class="sxs-lookup"><span data-stu-id="1f222-174">The automatic scaffolding shows each property in the model.</span></span>

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="1f222-176">Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="1f222-176">Creating the Edit View</span></span>

<span data-ttu-id="1f222-177">Přidejte následující `Edit` metoda domácí řadiče.</span><span class="sxs-lookup"><span data-stu-id="1f222-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="1f222-178">Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **upravit**.</span><span class="sxs-lookup"><span data-stu-id="1f222-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Přidání zobrazení upravit](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="1f222-180">Spusťte aplikaci a upravit první a poslední název jednoho uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f222-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="1f222-181">Pokud jste porušují `DataAnnotation` omezení, které se aplikovaly `UserModel` třída, při odeslání formuláře, zobrazí se chyby ověření, které jsou vytvářeny v serverovém kódu.</span><span class="sxs-lookup"><span data-stu-id="1f222-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="1f222-182">Například, pokud změníte jméno &quot;Ann&quot; k &quot;A&quot;, že při odeslání formuláře, na formuláři se zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="1f222-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="1f222-183">V tomto kurzu jste považuje uživatelské jméno jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="1f222-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="1f222-184">Proto nelze změnit vlastnost název uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f222-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="1f222-185">V *Edit.cshtml* souboru, hned za `Html.BeginForm` příkazu nastavte uživatelské jméno jako skryté políčko.</span><span class="sxs-lookup"><span data-stu-id="1f222-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="1f222-186">To způsobí, že vlastnost, která má být předán v modelu.</span><span class="sxs-lookup"><span data-stu-id="1f222-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="1f222-187">Následující fragment kódu ukazuje umístění `Hidden` příkaz:</span><span class="sxs-lookup"><span data-stu-id="1f222-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="1f222-188">Nahraďte `TextBoxFor` a `ValidationMessageFor` značek pro uživatelské jméno, `DisplayFor` volání.</span><span class="sxs-lookup"><span data-stu-id="1f222-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="1f222-189">`DisplayFor` Metoda zobrazí vlastnost jako element jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="1f222-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="1f222-190">Následující příklad ukazuje kód dokončené.</span><span class="sxs-lookup"><span data-stu-id="1f222-190">The following example shows the completed markup.</span></span> <span data-ttu-id="1f222-191">Původní `TextBoxFor` a `ValidationMessageFor` volání jsou vloženy do komentáře syntaxe Razor začít komentáře a end komentář znaků (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="1f222-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="1f222-192">Povolení ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="1f222-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="1f222-193">Pokud chcete povolit ověřování na straně klienta v architektuře ASP.NET MVC 3, je nutné nastavit dvěma příznaky a musí obsahovat tři soubory JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1f222-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="1f222-194">Otevřít aplikaci *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="1f222-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="1f222-195">Ověřte `that ClientValidationEnabled` a `UnobtrusiveJavaScriptEnabled` jsou nastaveny na hodnotu true v nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="1f222-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="1f222-196">Následující fragment z kořenového adresáře *Web.config* souboru zobrazuje správné nastavení:</span><span class="sxs-lookup"><span data-stu-id="1f222-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="1f222-197">Nastavení `UnobtrusiveJavaScriptEnabled` na hodnotu true, umožňuje nerušivý Ajax a ověření nerušivého klienta.</span><span class="sxs-lookup"><span data-stu-id="1f222-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="1f222-198">Použijete-li nerušivý ověřování, jsou pravidla ověřování převedena na atributy HTML5.</span><span class="sxs-lookup"><span data-stu-id="1f222-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="1f222-199">Názvy atributů jazyka HTML5 se může skládat jenom malá písmena, čísla a pomlčky.</span><span class="sxs-lookup"><span data-stu-id="1f222-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="1f222-200">Nastavení `ClientValidationEnabled` k ověřování na straně klienta true povoluje.</span><span class="sxs-lookup"><span data-stu-id="1f222-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="1f222-201">Nastavením těchto klíčů v aplikaci *Web.config* souboru, povolení ověření klienta a nerušivý JavaScript pro celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1f222-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="1f222-202">Můžete také povolit nebo zakázat tato nastavení v jednotlivých zobrazeních nebo metody kontroleru pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="1f222-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="1f222-203">Musíte taky zahrnout několik souborů JavaScript ve vykresleném zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1f222-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="1f222-204">Snadný způsob, jak zahrnout všechna zobrazení jazyka JavaScript je pro jejich přidávání k *Views\Shared\\_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="1f222-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="1f222-205">Nahraďte `<head>` element  *\_Layout.cshtml* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="1f222-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="1f222-206">První dva skripty jQuery jsou hostované pomocí Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="1f222-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="1f222-207">Využitím Microsoft Ajax CDN, může výrazně zlepšit výkon první podle aplikací.</span><span class="sxs-lookup"><span data-stu-id="1f222-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="1f222-208">Spusťte aplikaci a klikněte na odkaz pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="1f222-208">Run the application and click an edit link.</span></span> <span data-ttu-id="1f222-209">Zobrazení zdrojového kódu stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="1f222-209">View the page's source in the browser.</span></span> <span data-ttu-id="1f222-210">Zdroj prohlížeče ukazuje počet atributů formuláře `data-val` (pro ověření dat).</span><span class="sxs-lookup"><span data-stu-id="1f222-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="1f222-211">Pokud je povoleno ověření klienta a nerušivý JavaScript, obsahovat vstupní pole s pravidlem ověření klienta `data-val="true"` atributů ověření nerušivého klienta aktivovat.</span><span class="sxs-lookup"><span data-stu-id="1f222-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="1f222-212">Například `City` pole v modelu byla označených pomocí [požadované](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributů, což vede k HTML vidět v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="1f222-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="1f222-213">Pro každé pravidlo ověření klienta se přidá atribut, který má formulář `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="1f222-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="1f222-214">Pomocí `City` pole příkladu výše, vyžaduje ověření klienta pravidlo generuje `data-val-required` atribut a zpráva &quot;The města pole je povinné&quot;.</span><span class="sxs-lookup"><span data-stu-id="1f222-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="1f222-215">Spusťte aplikaci a úpravě jeden z uživatelů, zrušte `City` pole.</span><span class="sxs-lookup"><span data-stu-id="1f222-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="1f222-216">Kartě mimo pole zobrazí chybovou zprávu ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1f222-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Požadované města](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="1f222-218">Podobně, pro každý parametr v pravidle ověření klienta, přidání atributu má formuláře `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="1f222-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="1f222-219">Například `FirstName` vlastnost je opatřen poznámkou [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut a určuje minimální délku 3 a maximální délku 8.</span><span class="sxs-lookup"><span data-stu-id="1f222-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="1f222-220">Pravidlo ověření dat s názvem `length` má název parametru `max` a hodnota parametru 8.</span><span class="sxs-lookup"><span data-stu-id="1f222-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="1f222-221">Následující příklad zobrazuje HTML, který se vygeneruje pro `FirstName` pole při úpravách jeden z uživatelů:</span><span class="sxs-lookup"><span data-stu-id="1f222-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="1f222-222">Další informace o ověření nerušivého klienta, naleznete v příspěvku [Nerušivý ověření klienta v architektuře ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) v blogu Brada Wilsona.</span><span class="sxs-lookup"><span data-stu-id="1f222-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="1f222-223">V ASP.NET MVC 3 Beta budete muset někdy formulář odeslat, aby bylo možné spustit ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="1f222-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="1f222-224">To může být změněn pro finální verzi.</span><span class="sxs-lookup"><span data-stu-id="1f222-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="1f222-225">Vytvoření zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="1f222-225">Creating the Create View</span></span>

<span data-ttu-id="1f222-226">Dalším krokem je přidání `Create` metody akce a zobrazení, aby bylo možné zajistit, aby uživatel k vytvoření nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f222-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="1f222-227">Přidejte následující `Create` metoda domácí řadiče:</span><span class="sxs-lookup"><span data-stu-id="1f222-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="1f222-228">Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1f222-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Vytvoření zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="1f222-230">Spusťte aplikaci, vyberte **vytvořit** propojit, a přidání nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="1f222-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="1f222-231">`Create` Metoda automaticky využívá výhod ověření na straně klienta a na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="1f222-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="1f222-232">Zkuste zadat uživatelské jméno, které obsahuje prázdné znaky, jako například &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="1f222-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="1f222-233">Když kartě mimo pole uživatelského jména, chyba ověřování na straně klienta (`White space is not allowed`) se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="1f222-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="1f222-234">Přidejte metodu Delete</span><span class="sxs-lookup"><span data-stu-id="1f222-234">Add the Delete method</span></span>

<span data-ttu-id="1f222-235">K dokončení tohoto kurzu, přidejte následující `Delete` metoda domácí řadiče:</span><span class="sxs-lookup"><span data-stu-id="1f222-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="1f222-236">Přidat `Delete` zobrazení jako v předchozích krocích nastavení **zobrazit obsah** k **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="1f222-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Odstranit zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="1f222-238">Nyní máte jednoduchý, ale plně funkční aplikaci ASP.NET MVC 3 s ověřování.</span><span class="sxs-lookup"><span data-stu-id="1f222-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
