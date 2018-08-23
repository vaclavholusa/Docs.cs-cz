---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript | Dokumentace Microsoftu
author: microsoft
description: Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak snadné je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor. Ukázková aplikace s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: f60ca3e76fda8a18da1ad83e955e5e4759f5e3af
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757112"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="c4b4b-104">Vytváření MVC 3 aplikace s Razor a Nerušivý JavaScript</span><span class="sxs-lookup"><span data-stu-id="c4b4b-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="c4b4b-105">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c4b4b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c4b4b-106">Seznam uživatelů ukázkovou webovou aplikaci ukazuje, jak snadné je vytvoření aplikace ASP.NET MVC 3 pomocí zobrazovací modul Razor.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="c4b4b-107">Ukázková aplikace ukazuje, jak nový zobrazovací modul Razor s architekturou ASP.NET MVC verze 3 a Visual Studio 2010 použít k vytvoření fiktivní web seznamu uživatelů, který obsahuje funkce, jako je vytváření, zobrazení, úpravy a odstraňování uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="c4b4b-108">Tento kurz popisuje kroky, které byly provedeny, aby bylo možné vytvořit seznam uživatelů ukázkovou aplikaci ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="c4b4b-109">Projekt sady Visual Studio se zdrojovým kódem jazyka C# a VB je k dispozici v tomto tématu: [Stáhnout](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="c4b4b-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="c4b4b-110">Pokud máte dotazy týkající se v tomto kurzu, zveřejněte je [mvcforum](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4b4b-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="c4b4b-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="c4b4b-111">Overview</span></span>

<span data-ttu-id="c4b4b-112">Aplikace, kterou budete vytvářet je jednoduché uživatelské web seznamu.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="c4b4b-113">Uživatelům můžete zadat, zobrazit a aktualizovat informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-113">Users can enter, view, and update user information.</span></span>

![Ukázkový web](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="c4b4b-115">Můžete si stáhnout dokončený projekt jazyka Visual Basic a C# [tady](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="c4b4b-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="c4b4b-116">Vytvoření webové aplikace</span><span class="sxs-lookup"><span data-stu-id="c4b4b-116">Creating the Web Application</span></span>

<span data-ttu-id="c4b4b-117">Chcete-li zahájit kurz, otevřete Visual Studio 2010 a vytvořte nový projekt pomocí *webové aplikace ASP.NET MVC 3* šablony.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="c4b4b-118">Pojmenujte aplikaci &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="c4b4b-119">[![Nový projekt MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="c4b4b-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="c4b4b-120">V **nového projektu ASP.NET MVC 3** dialogového okna, vyberte **internetovou aplikaci**vyberte zobrazovací modul Razor a potom klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Dialogové okno nového projektu ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="c4b4b-122">V tomto kurzu nebudete používat poskytovatele členství prostředí ASP.NET, takže můžete odstranit všechny soubory přidružené k přihlášení a s členstvím.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="c4b4b-123">V **Průzkumníka řešení**, odebrat následující soubory a adresáře:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="c4b4b-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="c4b4b-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="c4b4b-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="c4b4b-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="c4b4b-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="c4b4b-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="c4b4b-127">*Views\Account* (a všechny soubory v tomto adresáři)</span><span class="sxs-lookup"><span data-stu-id="c4b4b-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="c4b4b-129">Upravit  <em>\_Layout.cshtml</em> souboru a nahraďte kód uvnitř `<div>` element s názvem `logindisplay` zprávou <em>&quot;</em>přihlašovací jméno zakázané&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="c4b4b-130">Následující příklad ukazuje novou značku:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="c4b4b-131">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="c4b4b-131">Adding the Model</span></span>

<span data-ttu-id="c4b4b-132">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *modely* složky, vyberte **přidat**a potom klikněte na **třídy**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Nová třída Mdl uživatele](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="c4b4b-134">Název třídy `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-134">Name the class `UserModel`.</span></span> <span data-ttu-id="c4b4b-135">Nahraďte obsah *UserModel* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="c4b4b-136">`UserModel` Třída reprezentuje uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="c4b4b-137">Každý člen třídy, je opatřen poznámkou [vyžaduje](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atribut z [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="c4b4b-138">Atributy v [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) obor názvů ověřování automatické a server straně klienta pro webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="c4b4b-139">Otevřít `HomeController` třídu a přidejte `using` direktiv, aby měli přístup ke `UserModel` a `Users` třídy:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="c4b4b-140">Hned za `HomeController` prohlášení, přidejte následující komentář a odkaz na `Users` třídy:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="c4b4b-141">`Users` Třída je úložiště dat zjednodušené, v paměti, které budete používat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="c4b4b-142">V reálné aplikaci byste použili databázi k ukládání informací o uživateli.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="c4b4b-143">Několik prvních řádků tohoto `HomeController` souboru jsou uvedeny v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="c4b4b-144">Sestavte aplikaci tak, aby model uživatelů budou mít k dispozici Průvodce generování uživatelského rozhraní v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="c4b4b-145">Vytváří se výchozí zobrazení</span><span class="sxs-lookup"><span data-stu-id="c4b4b-145">Creating the Default View</span></span>

<span data-ttu-id="c4b4b-146">Dalším krokem je přidání metody akce a zobrazení tak, aby uživatelé služby.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="c4b4b-147">Odstranit existující *Views\Home\Index* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="c4b4b-148">Vytvoří nový *Index* soubor k zobrazení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="c4b4b-149">V `HomeController` třídy, nahraďte obsah `Index` metodu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="c4b4b-150">Klepněte pravým tlačítkem myši `Index` metody a pak klikněte na tlačítko **přidat zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Přidání zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="c4b4b-152">Vyberte **vytvoření zobrazení se silnými typy** možnost.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="c4b4b-153">Pro **zobrazení dat třídy**vyberte **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="c4b4b-154">(Pokud se nezobrazí **Mvc3Razor.Models.UserModel** v **zobrazení dat třídy** pole, budete muset sestavit projekt.) Ujistěte se, že modul zobrazení nastavený na **Razor**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="c4b4b-155">Nastavte **zobrazit obsah** k **seznamu** a potom klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-155">Set **View content** to **List** and then click **Add**.</span></span>

![Přidání zobrazení Index](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="c4b4b-157">Nové zobrazení vygeneruje uživatelské automaticky rozhraní uživatelská data, která je předána `Index` zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="c4b4b-158">Prozkoumejte nově vygenerovaný *Views\Home\Index* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="c4b4b-159">**Vytvořit nový**, **upravit**, **podrobnosti**, a **odstranit** odkazy nefungují, ale zbývající části stránky je funkční.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="c4b4b-160">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-160">Run the page.</span></span> <span data-ttu-id="c4b4b-161">Zobrazí seznam uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-161">You see a list of users.</span></span>

![Indexová stránka](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="c4b4b-163">Otevřít *Index.cshtml* soubor a nahradit `ActionLink` kód pro **upravit**, **podrobnosti**, a **odstranit** následujícím kódem :</span><span class="sxs-lookup"><span data-stu-id="c4b4b-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="c4b4b-164">Uživatelské jméno se používá jako ID najít vybraný záznam v **upravit**, **podrobnosti**, a **odstranit** odkazy.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="c4b4b-165">Vytváření zobrazení podrobností</span><span class="sxs-lookup"><span data-stu-id="c4b4b-165">Creating the Details View</span></span>

<span data-ttu-id="c4b4b-166">Dalším krokem je přidání `Details` metody akce a zobrazení, aby bylo možné zobrazit podrobnosti o uživateli.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="c4b4b-168">Přidejte následující `Details` metodu pro kontroler home:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="c4b4b-169">Klepněte pravým tlačítkem myši `Details` metody a pak vyberte <strong>přidat zobrazení</strong>.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="c4b4b-170">Ověřte, že <strong>zobrazení dat třídy</strong> pole obsahuje <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="c4b4b-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="c4b4b-171">Nastavte <strong>zobrazit obsah</strong> k <strong>podrobnosti</strong> a potom klikněte na tlačítko <strong>přidat</strong>.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Přidat zobrazení podrobností](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="c4b4b-173">Spusťte aplikaci a vyberte odkaz podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-173">Run the application and select a details link.</span></span> <span data-ttu-id="c4b4b-174">Automatické generování uživatelského rozhraní zobrazuje každou vlastnost v modelu.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-174">The automatic scaffolding shows each property in the model.</span></span>

![Podrobnosti](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="c4b4b-176">Vytvoření zobrazení pro úpravy</span><span class="sxs-lookup"><span data-stu-id="c4b4b-176">Creating the Edit View</span></span>

<span data-ttu-id="c4b4b-177">Přidejte následující `Edit` metodu pro kontroler home.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="c4b4b-178">Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **upravit**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Přidání zobrazení pro úpravy](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="c4b4b-180">Spusťte aplikaci a upravte první a poslední název jednoho z uživatelů.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="c4b4b-181">Pokud jste porušují `DataAnnotation` omezení, které se použily `UserModel` třídy, při odeslání formuláře, zobrazí se chyby ověření, které jsou vytvářeny v kódu serveru.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="c4b4b-182">Například, pokud změníte křestní jméno &quot;Ann&quot; k &quot;A&quot;při odeslání formuláře, ve formuláři se zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="c4b4b-183">V tomto kurzu se zpracuje uživatelské jméno jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="c4b4b-184">Proto nelze změnit název vlastnosti uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="c4b4b-185">V *Edit.cshtml* souboru hned za `Html.BeginForm` prohlášení, nastavte uživatelské jméno jako skryté pole.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="c4b4b-186">To způsobí, že vlastnost, která má být předán v modelu.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="c4b4b-187">Následující fragment kódu ukazuje umístění `Hidden` – příkaz:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="c4b4b-188">Nahradit `TextBoxFor` a `ValidationMessageFor` značek pro uživatelské jméno `DisplayFor` volání.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="c4b4b-189">`DisplayFor` Metoda zobrazí vlastnosti jako prvek jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="c4b4b-190">Následující příklad ukazuje dokončené značky.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-190">The following example shows the completed markup.</span></span> <span data-ttu-id="c4b4b-191">Původní `TextBoxFor` a `ValidationMessageFor` volání, jsou zakomentované znaky začít komentář a konec komentáře syntaxe Razor (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="c4b4b-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="c4b4b-192">Povolení ověřování na straně klienta</span><span class="sxs-lookup"><span data-stu-id="c4b4b-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="c4b4b-193">Pokud chcete povolit ověřování na straně klienta v architektuře ASP.NET MVC 3, je nutné nastavit dva příznaky a musí obsahovat tři soubory jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="c4b4b-194">Otevřete aplikaci prvku *Web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="c4b4b-195">Ověřte `that ClientValidationEnabled` a `UnobtrusiveJavaScriptEnabled` jsou nastaveny na hodnotu true v nastavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="c4b4b-196">Následující fragment z kořenového adresáře *Web.config* souboru zobrazuje správné nastavení:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="c4b4b-197">Nastavení `UnobtrusiveJavaScriptEnabled` na hodnotu true umožňuje nerušivý Ajax a ověření nerušivého klienta.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="c4b4b-198">Použijete-li nerušivý ověření, ověřovacích pravidel jsou převedena na atributy HTML5.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="c4b4b-199">Názvy atributů HTML5 může být tvořen pouze malá písmena, číslice a spojovníky.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="c4b4b-200">Nastavení `ClientValidationEnabled` na hodnotu true umožňuje ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="c4b4b-201">Nastavením těchto klíčů v aplikaci *Web.config* souboru jste povolili ověřování na straně klienta a nerušivý JavaScript pro celou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="c4b4b-202">Můžete také povolit nebo zakázat tato nastavení v jednotlivých zobrazeních nebo v metodách kontroleru pomocí následujícího kódu:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="c4b4b-203">Také musíte zahrnout několik souborů JavaScriptu vykreslené zobrazení.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="c4b4b-204">Snadný způsob, jak ve všech zobrazeních přidejte kód JavaScript je pro jejich přidání do *Views\Shared\\_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="c4b4b-205">Nahradit `<head>` elementu  *\_Layout.cshtml* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="c4b4b-206">První dva skripty jQuery jsou hostované ve Microsoft Ajax Content Delivery Network (CDN).</span><span class="sxs-lookup"><span data-stu-id="c4b4b-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="c4b4b-207">S využitím Microsoft Ajax CDN může výrazně zlepšit výkon stiskněte první aplikací.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="c4b4b-208">Spusťte aplikaci a klikněte na odkaz pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-208">Run the application and click an edit link.</span></span> <span data-ttu-id="c4b4b-209">Zobrazte zdroj stránky v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-209">View the page's source in the browser.</span></span> <span data-ttu-id="c4b4b-210">Zdrojového kódu prohlížeče ukazuje mnoho atributů ve formátu `data-val` (pro ověření dat).</span><span class="sxs-lookup"><span data-stu-id="c4b4b-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="c4b4b-211">Pokud je povoleno ověřování na straně klienta a nerušivý JavaScript, obsahovat vstupní pole pomocí pravidla ověřování na straně klienta `data-val="true"` atribut k aktivaci ověření nerušivého klienta.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="c4b4b-212">Například `City` byl doplněn pole v modelu [vyžaduje](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) atributů, což vede k HTML je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="c4b4b-213">Pro každé pravidlo ověřování na straně klienta se přidá atribut, který má tvar `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="c4b4b-214">Pomocí `City` pole uvedená výše, generuje pravidlo vyžaduje ověřování na straně klienta, například `data-val-required` atribut a zpráva &quot;je povinné pole Město&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="c4b4b-215">Spusťte aplikaci a úpravě jednoho z uživatelů, zrušte `City` pole.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="c4b4b-216">Když kartě mimo pole se zobrazí chybovou zprávu ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Město vyžaduje](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="c4b4b-218">Podobně pro každý parametr pravidla ověřování na straně klienta, přidání atributu, který má tvar `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="c4b4b-219">Například `FirstName` vlastnost je opatřen poznámkou [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atribut a určuje minimální délku 3 a maximální délce 8.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="c4b4b-220">Pravidlo ověřování dat s názvem `length` má název parametru `max` a hodnota parametru 8.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="c4b4b-221">Následující příklad zobrazuje kód HTML, který je generován pro `FirstName` pole při úpravě jednoho z uživatelů:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="c4b4b-222">Další informace o ověření nerušivého klienta naleznete v příspěvku [Nerušivý ověření klienta v architektuře ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) v blogu Brada Wilsona.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="c4b4b-223">V ASP.NET MVC 3 Beta musíte někdy odesláním formuláře, aby bylo možné spustit ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="c4b4b-224">To může být změněn ve finální verzi.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="c4b4b-225">Vytvoření zobrazení pro vytváření</span><span class="sxs-lookup"><span data-stu-id="c4b4b-225">Creating the Create View</span></span>

<span data-ttu-id="c4b4b-226">Dalším krokem je přidání `Create` metody akce a zobrazení, chcete-li povolit uživatelům vytvoření nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="c4b4b-227">Přidejte následující `Create` metodu pro kontroler home:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="c4b4b-228">Přidání zobrazení jako v předchozích krocích, ale nastavit **zobrazit obsah** k **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Vytvoření zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="c4b4b-230">Spusťte aplikaci, vyberte **vytvořit** propojit, a přidejte nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="c4b4b-231">`Create` Metoda automaticky využívá výhod ověření na straně klienta i stranu serveru.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="c4b4b-232">Zkuste zadat uživatelské jméno, který obsahuje prázdné znaky, jako například &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="c4b4b-233">Pokud kartu mimo pole uživatelské jméno na chybu ověřování na straně klienta (`White space is not allowed`) se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="c4b4b-234">Přidání metody odstranění</span><span class="sxs-lookup"><span data-stu-id="c4b4b-234">Add the Delete method</span></span>

<span data-ttu-id="c4b4b-235">Pro absolvování tohoto kurzu, přidejte následující `Delete` metodu pro kontroler home:</span><span class="sxs-lookup"><span data-stu-id="c4b4b-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="c4b4b-236">Přidat `Delete` zobrazení jako v předchozích krocích, nastavení **zobrazit obsah** k **odstranit**.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Odstranit zobrazení](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="c4b4b-238">Nyní máte jednoduchý, ale plně funkční aplikaci ASP.NET MVC 3 s ověřením.</span><span class="sxs-lookup"><span data-stu-id="c4b4b-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
