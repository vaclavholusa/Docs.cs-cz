---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: '7. část: Členství a ověřování | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 7. část se věnuje členství a autorizace.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757104"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="95805-104">7. část: Členství a ověřování</span><span class="sxs-lookup"><span data-stu-id="95805-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="95805-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="95805-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="95805-106">MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="95805-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="95805-107">Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="95805-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="95805-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="95805-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="95805-109">7. část se věnuje členství a autorizace.</span><span class="sxs-lookup"><span data-stu-id="95805-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="95805-110">Správce Store kontroleru je nyní k dispozici všem uživatelům na našem webu.</span><span class="sxs-lookup"><span data-stu-id="95805-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="95805-111">Změňme ji omezit oprávnění na správce webu.</span><span class="sxs-lookup"><span data-stu-id="95805-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="95805-112">Přidání zobrazení a AccountController</span><span class="sxs-lookup"><span data-stu-id="95805-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="95805-113">Jedním z rozdílů mezi kompletní šablonu webové aplikace ASP.NET MVC 3 a šablony ASP.NET MVC 3 prázdná webová aplikace je, že prázdnou šablonu neobsahuje řadič účet.</span><span class="sxs-lookup"><span data-stu-id="95805-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="95805-114">Přidáme řadič účet tak, že zkopírujete několik souborů z nové aplikace ASP.NET MVC vytvořené z úplné šablony webové aplikace ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="95805-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="95805-115">Vytvoření nové aplikace ASP.NET MVC pomocí úplné šablony webové aplikace ASP.NET MVC 3 a zkopírujte následující soubory do stejného adresáře v našem projektu:</span><span class="sxs-lookup"><span data-stu-id="95805-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="95805-116">Zkopírujte AccountController.cs v adresáři řadiče</span><span class="sxs-lookup"><span data-stu-id="95805-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="95805-117">Zkopírujte AccountModels v adresáři modelů</span><span class="sxs-lookup"><span data-stu-id="95805-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="95805-118">Vytvořte účet adresář do adresáře zobrazení a zkopírujte všechny čtyři zobrazení v</span><span class="sxs-lookup"><span data-stu-id="95805-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="95805-119">Změna oboru názvů pro třídy Kontroleru a Model, začnou se MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="95805-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="95805-120">Třída AccountController by měl používat obor názvů MvcMusicStore.Controllers a třída AccountModels by měl používat obor názvů MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="95805-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="95805-121">*Poznámka: Tyto soubory jsou také k dispozici MvcMusicStore Assets.zip soubor ke stažení, ze kterého jsme naše lokality návrhu soubory zkopírovány na začátku tohoto kurzu. Členství soubory jsou umístěny v adresáři kódu.*</span><span class="sxs-lookup"><span data-stu-id="95805-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="95805-122">Aktualizované řešení by měl vypadat nějak takto:</span><span class="sxs-lookup"><span data-stu-id="95805-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="95805-123">Přidání správce s webem konfigurace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="95805-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="95805-124">Předtím, než jsme v našem webu vyžadují autorizace, potřebujeme vytvořit uživatele s přístupem.</span><span class="sxs-lookup"><span data-stu-id="95805-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="95805-125">Nejjednodušší způsob, jak vytvořit uživatele je použití webu integrované konfigurace technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95805-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="95805-126">Kliknutím na ikonu v okně Průzkumník řešení po spuštění webu konfigurace technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="95805-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="95805-127">Tím se spustí konfigurace webu.</span><span class="sxs-lookup"><span data-stu-id="95805-127">This launches a configuration website.</span></span> <span data-ttu-id="95805-128">Klikněte na kartu zabezpečení na domovské obrazovce a pak klikněte na odkaz "Povolit role" na střed obrazovky.</span><span class="sxs-lookup"><span data-stu-id="95805-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="95805-129">Klikněte na odkaz "Vytvořit nebo spravovat role".</span><span class="sxs-lookup"><span data-stu-id="95805-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="95805-130">Zadejte "Administrator" jako název role a klikněte na tlačítko Přidat roli.</span><span class="sxs-lookup"><span data-stu-id="95805-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="95805-131">Klikněte na tlačítko Zpět a potom klikněte na odkaz vytvořit uživatele na levé straně.</span><span class="sxs-lookup"><span data-stu-id="95805-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="95805-132">Vyplňte pole informace uživatele na levé straně pomocí následujících informací:</span><span class="sxs-lookup"><span data-stu-id="95805-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="95805-133">**Pole**</span><span class="sxs-lookup"><span data-stu-id="95805-133">**Field**</span></span> | <span data-ttu-id="95805-134">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="95805-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="95805-135">**Uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="95805-135">**User Name**</span></span> | <span data-ttu-id="95805-136">Správce</span><span class="sxs-lookup"><span data-stu-id="95805-136">Administrator</span></span> |
| <span data-ttu-id="95805-137">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="95805-137">**Password**</span></span> | <span data-ttu-id="95805-138">/ password123!</span><span class="sxs-lookup"><span data-stu-id="95805-138">password123!</span></span> |
| <span data-ttu-id="95805-139">**Potvrzení hesla**</span><span class="sxs-lookup"><span data-stu-id="95805-139">**Confirm Password**</span></span> | <span data-ttu-id="95805-140">/ password123!</span><span class="sxs-lookup"><span data-stu-id="95805-140">password123!</span></span> |
| <span data-ttu-id="95805-141">**E-mailu**</span><span class="sxs-lookup"><span data-stu-id="95805-141">**E-mail**</span></span> | <span data-ttu-id="95805-142">(žádné e-mailová adresa bude fungovat)</span><span class="sxs-lookup"><span data-stu-id="95805-142">(any email address will work)</span></span> |
| <span data-ttu-id="95805-143">**Bezpečnostní otázku**</span><span class="sxs-lookup"><span data-stu-id="95805-143">**Security Question**</span></span> | <span data-ttu-id="95805-144">(cokoli, co chcete)</span><span class="sxs-lookup"><span data-stu-id="95805-144">(whatever you like)</span></span> |
| <span data-ttu-id="95805-145">**Zabezpečovací odpověď**</span><span class="sxs-lookup"><span data-stu-id="95805-145">**Security Answer**</span></span> | <span data-ttu-id="95805-146">(cokoli, co chcete)</span><span class="sxs-lookup"><span data-stu-id="95805-146">(whatever you like)</span></span> |

<span data-ttu-id="95805-147">*Poznámka: Můžete samozřejmě používat jakékoli heslo, které chcete. Výchozí nastavení zabezpečení hesel vyžadují heslo, které má 7 znaků a obsahuje jeden jiný než alfanumerický znak.*</span><span class="sxs-lookup"><span data-stu-id="95805-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="95805-148">Vyberte roli správce u tohoto uživatele a klikněte na tlačítko Vytvořit uživatele.</span><span class="sxs-lookup"><span data-stu-id="95805-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="95805-149">V tomto okamžiku byste měli vidět zprávu s oznámením, že uživatel byl úspěšně vytvořen.</span><span class="sxs-lookup"><span data-stu-id="95805-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="95805-150">Teď můžete zavřít okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="95805-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="95805-151">Autorizace na základě rolí</span><span class="sxs-lookup"><span data-stu-id="95805-151">Role-based Authorization</span></span>

<span data-ttu-id="95805-152">Nyní jsme můžete omezit přístup k určení, že uživatel musí být v roli správce pro přístup k žádné akci kontroleru ve třídě StoreManagerController pomocí atributu [Authorize].</span><span class="sxs-lookup"><span data-stu-id="95805-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="95805-153">*Poznámka: Atribut [Authorize] lze umístit na konkrétní akci metody i na úrovni třídy Kontroleru.*</span><span class="sxs-lookup"><span data-stu-id="95805-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="95805-154">Teď přejdete do /StoreManager otevře dialogové okno přihlášení:</span><span class="sxs-lookup"><span data-stu-id="95805-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="95805-155">Po přihlášení se náš nový účet správce, jsme byli schopni přejděte na obrazovku alba upravit jako před.</span><span class="sxs-lookup"><span data-stu-id="95805-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="95805-156">[Předchozí](mvc-music-store-part-6.md)
> [další](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="95805-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
