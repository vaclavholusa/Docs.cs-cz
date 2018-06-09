---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Změnit primární klíč pro uživatele v identitě ASP.NET Identity | Microsoft Docs
author: tfitzmac
description: Ve Visual Studiu 2013 se výchozí webová aplikace používá hodnotu řetězce pro klíč pro uživatelské účty. ASP.NET Identity umožňuje změnit typ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26563773"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="e9cd1-104">Změnit primární klíč pro uživatele v identitě ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="e9cd1-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="e9cd1-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e9cd1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e9cd1-106">Ve Visual Studiu 2013 se výchozí webová aplikace používá hodnotu řetězce pro klíč pro uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="e9cd1-107">ASP.NET Identity umožňuje změnit typ klíče podle požadavků vaší data.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="e9cd1-108">Můžete například změnit typ klíče z řetězce na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="e9cd1-109">Toto téma ukazuje, jak spustit s výchozí webové aplikace a změňte klíč účtu uživatele na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="e9cd1-110">Stejné změny můžete použít k implementaci libovolného typu klíč ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="e9cd1-111">Ukazuje, jak tyto změny provést ve výchozím nastavení webové aplikace, ale může použít podobné úpravy vlastní aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="e9cd1-112">Zobrazuje změny potřebné při práci s MVC nebo webového formuláře.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e9cd1-113">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="e9cd1-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e9cd1-114">Visual Studio 2013 s aktualizací 2 (nebo novější)</span><span class="sxs-lookup"><span data-stu-id="e9cd1-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="e9cd1-115">ASP.NET Identity 2.1 nebo novější</span><span class="sxs-lookup"><span data-stu-id="e9cd1-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="e9cd1-116">K provedení kroků v tomto kurzu, musí mít Visual Studio 2013 Update 2 (nebo novější) a webové aplikace vytvořené z šablony webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="e9cd1-117">Šablona změnit ve verzi Update 3.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-117">The template changed in Update 3.</span></span> <span data-ttu-id="e9cd1-118">Toto téma ukazuje, jak změnit šablonu v Update 2 a Update 3.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="e9cd1-119">Toto téma obsahuje následující oddíly:</span><span class="sxs-lookup"><span data-stu-id="e9cd1-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e9cd1-120">Změnit typ klíče v třídu Identity uživatelů</span><span class="sxs-lookup"><span data-stu-id="e9cd1-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="e9cd1-121">Přidat vlastní třídy Identity, které používají typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="e9cd1-122">Změna kontextu třídy a uživatel správce chcete použít typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="e9cd1-123">Změna konfigurace spuštění používat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="e9cd1-124">Pro MVC s aktualizací 2 změňte AccountController předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="e9cd1-125">Pro MVC s aktualizací 3 změňte AccountController a ManageController předávat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="e9cd1-126">Pro webových formulářů s aktualizací 2 změňte účet stránky předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="e9cd1-127">Pro webových formulářů s aktualizací 3 změňte účet stránky předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="e9cd1-128">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e9cd1-128">Run application</span></span>](#run)
- [<span data-ttu-id="e9cd1-129">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="e9cd1-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="e9cd1-130">Změnit typ klíče v třídu Identity uživatelů</span><span class="sxs-lookup"><span data-stu-id="e9cd1-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="e9cd1-131">Ve vašem projektu vytvořené z šablony webové aplikace ASP.NET určíte, že třídě ApplicationUser používat celé pro klíč pro uživatelské účty.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="e9cd1-132">IdentityModels.cs, změňte třídě ApplicationUser dědění z IdentityUser, která má typ **int** pro obecný parametr TKey.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="e9cd1-133">Také předáte názvy tři vlastní třídy, které nebyly dosud implementována.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="e9cd1-134">Změnili jste typ klíče, ale ve výchozím nastavení, zbývající aplikace stále předpokládá, že klíč je řetězec.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="e9cd1-135">Je třeba explicitně určit typ klíče v kódu, který předpokládá řetězec.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="e9cd1-136">V **ApplicationUser** třídy, změňte **GenerateUserIdentityAsync** tak, aby zahrnoval int, jak je znázorněno v následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="e9cd1-137">Tato změna není nutné u projektů webové formuláře se šablonou Update 3.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="e9cd1-138">Přidat vlastní třídy Identity, které používají typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="e9cd1-139">Další Identity třídy, jako je například IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, objektu UserStore, úložiště RoleStore, jsou stále nastavit tak, aby použije klíč pro řetězec.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="e9cd1-140">Vytvořte nové verze těchto tříd, které zadejte celé číslo pro klíč.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="e9cd1-141">Není potřeba poskytnout mnohem implementaci kódu v těchto tříd, především právě nastavujete int jako klíč.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="e9cd1-142">IdentityModels.cs souboru přidejte následující třídy.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="e9cd1-143">Změna kontextu třídy a uživatel správce chcete použít typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="e9cd1-144">V IdentityModels.cs, změňte definici **ApplicationDbContext** přizpůsobit třídu do nové třídy a **int** klíče, jak je znázorněno v zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="e9cd1-145">Parametr ThrowIfV1Schema již není platný v konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="e9cd1-146">Změňte konstruktoru, nesplňuje ThrowIfV1Schema hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="e9cd1-147">Otevřete IdentityConfig.cs a změňte **ApplicationUserManger** třídu se má použít nový uživatel uložit třídu pro zachování dat a **int** klíče.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="e9cd1-148">V šabloně Update 3 musíte změnit ApplicationSignInManager třídy.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="e9cd1-149">Změna konfigurace spuštění používat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="e9cd1-150">V Startup.Auth.cs nahraďte kód funkce OnValidateIdentity, jak je znázorněno dole.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="e9cd1-151">Všimněte si, že definici getUserIdCallback analyzuje hodnotu řetězce na celé číslo.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="e9cd1-152">Pokud váš projekt nerozpoznává obecnou implementaci **reprezentuje GetUserId** metody, budete muset aktualizovat balíček ASP.NET Identity NuGet s verzí 2.1</span><span class="sxs-lookup"><span data-stu-id="e9cd1-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="e9cd1-153">Provedli jste mnoho změn infrastruktury třídy používané ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="e9cd1-154">Pokud se pokusíte kompilace projektu, si všimnete mnoho chyb.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="e9cd1-155">Naštěstí zbývající chyby jsou podobné.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="e9cd1-156">Identity – třída očekává celé číslo klíče, ale řadič (nebo webového formuláře) je předávání hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="e9cd1-157">V každém případě je potřeba převést řetězec na a celé číslo při volání **reprezentuje GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="e9cd1-158">Můžete buď fungovat prostřednictvím z kompilace v seznamu chyb nebo postupujte podle níže změny.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="e9cd1-159">Zbývající změny závisí na typu projektu vytváříte a aktualizace, které jste nainstalovali v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="e9cd1-160">Můžete přejít přímo na odpovídající část prostřednictvím následujících odkazů</span><span class="sxs-lookup"><span data-stu-id="e9cd1-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="e9cd1-161">Pro MVC s aktualizací 2 změňte AccountController předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="e9cd1-162">Pro MVC s aktualizací 3 změňte AccountController a ManageController předávat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="e9cd1-163">Pro webových formulářů s aktualizací 2 změňte účet stránky předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="e9cd1-164">Pro webových formulářů s aktualizací 3 změňte účet stránky předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="e9cd1-165">Pro MVC s aktualizací 2 změňte AccountController předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="e9cd1-166">Otevřete soubor AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="e9cd1-167">Budete muset změnit následující metody.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-167">You need to change the following methods.</span></span>

<span data-ttu-id="e9cd1-168">**ConfirmEmail** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="e9cd1-169">**Zrušit přidružení** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="e9cd1-170">**Manage(ManageUserViewModel)** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="e9cd1-171">**LinkLoginCallback** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="e9cd1-172">**RemoveAccountList** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="e9cd1-173">**HasPassword** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="e9cd1-174">Teď můžete [spusťte aplikaci](#run) a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="e9cd1-175">Pro MVC s aktualizací 3 změňte AccountController a ManageController předávat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="e9cd1-176">Otevřete soubor AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="e9cd1-177">Budete muset změnit metodu.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-177">You need to change the following method.</span></span>

<span data-ttu-id="e9cd1-178">**ConfirmEmail** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="e9cd1-179">**SendCode** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="e9cd1-180">Otevřete soubor ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="e9cd1-181">Budete muset změnit následující metody.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-181">You need to change the following methods.</span></span>

<span data-ttu-id="e9cd1-182">**Index** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="e9cd1-183">**RemoveLogin** metody</span><span class="sxs-lookup"><span data-stu-id="e9cd1-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="e9cd1-184">**AddPhoneNumber** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="e9cd1-185">**EnableTwoFactorAuthentication** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="e9cd1-186">**DisableTwoFactorAuthentication** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="e9cd1-187">**VerifyPhoneNumber** metody</span><span class="sxs-lookup"><span data-stu-id="e9cd1-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="e9cd1-188">**RemovePhoneNumber** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="e9cd1-189">**Změna hesla byla** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="e9cd1-190">**SetPassword** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="e9cd1-191">**ManageLogins** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="e9cd1-192">**LinkLoginCallback** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="e9cd1-193">**HasPassword** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="e9cd1-194">**HasPhoneNumber** – metoda</span><span class="sxs-lookup"><span data-stu-id="e9cd1-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="e9cd1-195">Teď můžete [spusťte aplikaci](#run) a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="e9cd1-196">Pro webových formulářů s aktualizací 2 změňte účet stránky předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="e9cd1-197">Pro webových formulářů s aktualizací 2 budete muset změnit na následujících stránkách.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="e9cd1-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="e9cd1-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="e9cd1-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="e9cd1-201">Teď můžete [spusťte aplikaci](#run) a registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="e9cd1-202">Pro webových formulářů s aktualizací 3 změňte účet stránky předat typ klíče</span><span class="sxs-lookup"><span data-stu-id="e9cd1-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="e9cd1-203">Pro webových formulářů s aktualizací 3 budete muset změnit na následujících stránkách.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="e9cd1-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="e9cd1-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="e9cd1-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="e9cd1-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="e9cd1-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="e9cd1-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="e9cd1-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="e9cd1-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="e9cd1-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="e9cd1-212">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="e9cd1-212">Run application</span></span>

<span data-ttu-id="e9cd1-213">Dokončení všechny požadované změny pro výchozí šablony webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="e9cd1-214">Spusťte aplikaci a zaregistrovat nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-214">Run the application and register a new user.</span></span> <span data-ttu-id="e9cd1-215">Po registraci uživatele, si všimnete, že AspNetUsers tabulka obsahuje sloupec Id, který je celé číslo.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nový primární klíč](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="e9cd1-217">Pokud jste předtím vytvořili ASP.NET Identity tabulky pomocí jiného primárního klíče, budete muset provést další změny.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="e9cd1-218">Pokud je to možné odstraňte stávající databázi.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="e9cd1-219">Databáze bude znovu vytvořena s správné návrhu při spuštění webové aplikace a přidat nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="e9cd1-220">Pokud se odstranění není možné, spustit migrace code first, chcete-li změnit tabulky.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="e9cd1-221">Ale nový primární klíč celé číslo nebude nastavit jako vlastnost SQL IDENTITY v databázi.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="e9cd1-222">Je nutné ručně nastavit sloupec Id jako IDENTITY.</span><span class="sxs-lookup"><span data-stu-id="e9cd1-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="e9cd1-223">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="e9cd1-223">Other resources</span></span>

- [<span data-ttu-id="e9cd1-224">Přehled poskytovatelů vlastního úložiště pro ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="e9cd1-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="e9cd1-225">Migrace stávajícího webu z členství SQL na ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="e9cd1-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="e9cd1-226">Migrace dat Universal zprostředkovatele členství a uživatelské profily mají být ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="e9cd1-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="e9cd1-227">[Ukázková aplikace](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) se změněné primárním klíčem</span><span class="sxs-lookup"><span data-stu-id="e9cd1-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
