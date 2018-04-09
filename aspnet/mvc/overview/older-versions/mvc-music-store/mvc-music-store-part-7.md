---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Část 7: Členství a autorizace | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 7 popisuje členství a autorizaci.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="a0fbe-104">Část 7: Členství a autorizace</span><span class="sxs-lookup"><span data-stu-id="a0fbe-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="a0fbe-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a0fbe-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a0fbe-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a0fbe-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a0fbe-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a0fbe-109">Část 7 popisuje členství a autorizaci.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="a0fbe-110">Správce obchodu kontroleru je nyní k dispozici všem uživatelům, kteří navštěvují náš web.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="a0fbe-111">Umožňuje změnit to omezit oprávnění na správce webu.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="a0fbe-112">Přidání zobrazení a AccountController</span><span class="sxs-lookup"><span data-stu-id="a0fbe-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="a0fbe-113">Jeden rozdíl mezi kompletní šablonou webové aplikace ASP.NET MVC 3 a šablony ASP.NET MVC 3 prázdný webové aplikace je, že prázdné šablonu neobsahuje řadič účet.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="a0fbe-114">Vytvoření účtu řadiče přidáme zkopírováním několik souborů z nové aplikace ASP.NET MVC z úplné šablony webové aplikace ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="a0fbe-115">Vytvoření nové aplikace ASP.NET MVC pomocí úplné šablony webové aplikace ASP.NET MVC 3 a zkopírujte následující soubory do stejných adresářích v našem projektu:</span><span class="sxs-lookup"><span data-stu-id="a0fbe-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="a0fbe-116">Zkopírujte AccountController.cs v adresáři řadiče</span><span class="sxs-lookup"><span data-stu-id="a0fbe-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="a0fbe-117">Zkopírujte AccountModels v adresáři modely</span><span class="sxs-lookup"><span data-stu-id="a0fbe-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="a0fbe-118">Vytvořte adresář účet uvnitř zobrazení adresáře a zkopírujte všechny čtyři zobrazení</span><span class="sxs-lookup"><span data-stu-id="a0fbe-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="a0fbe-119">Změňte obor názvů pro třídy Controller a modelu, tak budou začínat MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="a0fbe-120">Třída AccountController by měl použít obor názvů MvcMusicStore.Controllers a třída AccountModels by měl použít obor názvů MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="a0fbe-121">*Poznámka: Tyto soubory jsou také k dispozici při stahování MvcMusicStore Assets.zip, ze kterého jsme zkopírovat soubory návrhu naše lokality na začátku tohoto kurzu. Členství soubory jsou umístěny v adresáři kódu.*</span><span class="sxs-lookup"><span data-stu-id="a0fbe-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="a0fbe-122">Aktualizované řešení by měl vypadat asi takto:</span><span class="sxs-lookup"><span data-stu-id="a0fbe-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="a0fbe-123">Přidání správce s lokalitou konfigurace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a0fbe-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="a0fbe-124">Než budeme autorizace vyžadovat v našem webu, budeme potřebovat pro vytvoření uživatele s přístupem.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="a0fbe-125">Nejjednodušší způsob, jak vytvořit uživateli se má používat integrované konfigurace ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="a0fbe-126">Konfigurace technologie ASP.NET Web spusťte kliknutím na následující ikona v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="a0fbe-127">Spustí se konfigurace webu.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-127">This launches a configuration website.</span></span> <span data-ttu-id="a0fbe-128">Klikněte na kartě zabezpečení na domovské obrazovce a potom klikněte na odkaz "Povolit role" v centru obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="a0fbe-129">Klikněte na odkaz "Vytvořit nebo spravovat role".</span><span class="sxs-lookup"><span data-stu-id="a0fbe-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="a0fbe-130">Zadejte název role "Správce" a klikněte na tlačítko Přidat roli.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="a0fbe-131">Klikněte na tlačítko Zpět a potom klikněte na odkaz vytvořit uživatele na levé straně.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="a0fbe-132">Vyplňte pole informace uživatele na levé straně pomocí následujících informací:</span><span class="sxs-lookup"><span data-stu-id="a0fbe-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="a0fbe-133">**Pole**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-133">**Field**</span></span> | <span data-ttu-id="a0fbe-134">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="a0fbe-135">**Uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-135">**User Name**</span></span> | <span data-ttu-id="a0fbe-136">Správce</span><span class="sxs-lookup"><span data-stu-id="a0fbe-136">Administrator</span></span> |
| <span data-ttu-id="a0fbe-137">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-137">**Password**</span></span> | <span data-ttu-id="a0fbe-138">password123!</span><span class="sxs-lookup"><span data-stu-id="a0fbe-138">password123!</span></span> |
| <span data-ttu-id="a0fbe-139">**Potvrzení hesla**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-139">**Confirm Password**</span></span> | <span data-ttu-id="a0fbe-140">password123!</span><span class="sxs-lookup"><span data-stu-id="a0fbe-140">password123!</span></span> |
| <span data-ttu-id="a0fbe-141">**E-mail**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-141">**E-mail**</span></span> | <span data-ttu-id="a0fbe-142">(všechny e-mailová adresa bude fungovat.)</span><span class="sxs-lookup"><span data-stu-id="a0fbe-142">(any email address will work)</span></span> |
| <span data-ttu-id="a0fbe-143">**Bezpečnostní otázku**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-143">**Security Question**</span></span> | <span data-ttu-id="a0fbe-144">(ať je třeba)</span><span class="sxs-lookup"><span data-stu-id="a0fbe-144">(whatever you like)</span></span> |
| <span data-ttu-id="a0fbe-145">**Zabezpečení odpovědí**</span><span class="sxs-lookup"><span data-stu-id="a0fbe-145">**Security Answer**</span></span> | <span data-ttu-id="a0fbe-146">(ať je třeba)</span><span class="sxs-lookup"><span data-stu-id="a0fbe-146">(whatever you like)</span></span> |

<span data-ttu-id="a0fbe-147">*Poznámka: Samozřejmě můžete žádné heslo, které si přejete. Výchozí nastavení zabezpečení heslo vyžadovat zadání hesla, která má délku 7 znaků dlouhé a obsahuje jeden jiný než alfanumerický znak.*</span><span class="sxs-lookup"><span data-stu-id="a0fbe-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="a0fbe-148">Vyberte roli správce pro tohoto uživatele a klikněte na tlačítko Vytvořit uživatele.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="a0fbe-149">V tomto okamžiku zobrazí zprávu s upozorněním, že byl uživatel vytvořen úspěšně.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="a0fbe-150">Teď můžete zavřít okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="a0fbe-151">Ověřování na základě rolí</span><span class="sxs-lookup"><span data-stu-id="a0fbe-151">Role-based Authorization</span></span>

<span data-ttu-id="a0fbe-152">Nyní jsme můžete omezit přístup k určení, že uživatel musí být v roli správce pro přístup k žádné akci kontroleru ve třídě StoreManagerController pomocí atributu [Authorize].</span><span class="sxs-lookup"><span data-stu-id="a0fbe-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="a0fbe-153">*Poznámka: Atribut [autorizovat] můžete umístit na konkrétní akce metody i na úrovni třídy Kontroleru.*</span><span class="sxs-lookup"><span data-stu-id="a0fbe-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="a0fbe-154">Nyní procházení k /StoreManager zobrazí dialogové okno přihlášení:</span><span class="sxs-lookup"><span data-stu-id="a0fbe-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="a0fbe-155">Po přihlášení s naše nový účet správce se nám moci přejít na obrazovce upravit Album jako před.</span><span class="sxs-lookup"><span data-stu-id="a0fbe-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0fbe-156">[Předchozí](mvc-music-store-part-6.md)
> [další](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="a0fbe-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
