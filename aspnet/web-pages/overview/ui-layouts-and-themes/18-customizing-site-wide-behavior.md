---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Přizpůsobení chování na webu pro ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Tato kapitola vysvětluje, jak nakonfigurovat nastavení pro celý web nebo celou složku, nikoli jen na stránce.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: 4457318bcf1d2886eb8ed68fdd795eea7905368b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a><span data-ttu-id="ae975-103">Přizpůsobení chování na webu pro ASP.NET – webové stránky (Razor) servery</span><span class="sxs-lookup"><span data-stu-id="ae975-103">Customizing Site-Wide Behavior for ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="ae975-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ae975-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ae975-105">Tento článek vysvětluje, jak nakonfigurovat nastavení na straně serveru pro stránky na webu technologie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="ae975-105">This article explains how to make site-side settings for pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="ae975-106">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="ae975-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="ae975-107">Jak spustit kód který vám umožní sadu hodnot (globální hodnot nebo nastavení pomocníka) pro všechny stránky v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="ae975-107">How to run code that lets you set values (global values or helper settings) for all pages in a site.</span></span>
> - <span data-ttu-id="ae975-108">Jak spustit kód, který umožňuje nastavit hodnoty pro všechny stránky ve složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-108">How to run code that lets you set values for all pages in a folder.</span></span>
> - <span data-ttu-id="ae975-109">Postup spuštění kódu před a po každém načte.</span><span class="sxs-lookup"><span data-stu-id="ae975-109">How to run code before and after a page loads.</span></span>
> - <span data-ttu-id="ae975-110">Postup odeslání chyb při centrální chybovou stránku.</span><span class="sxs-lookup"><span data-stu-id="ae975-110">How to send errors to a central error page.</span></span>
> - <span data-ttu-id="ae975-111">Postup přidání ověřování pro všechny stránky ve složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-111">How to add authentication to all pages in a folder.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ae975-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="ae975-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ae975-113">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="ae975-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="ae975-114">Služba WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="ae975-114">WebMatrix 3</span></span>
> - <span data-ttu-id="ae975-115">Knihovnu ASP.NET Web Helpers (balíček NuGet)</span><span class="sxs-lookup"><span data-stu-id="ae975-115">ASP.NET Web Helpers Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="ae975-116">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 3 a Visual Studio 2013 (nebo Visual Studio Express 2013 pro Web), s výjimkou je nemohou používat knihovnu ASP.NET Web Helpers.</span><span class="sxs-lookup"><span data-stu-id="ae975-116">This tutorial also works with ASP.NET Web Pages 3 and Visual Studio 2013 (or Visual Studio Express 2013 for Web), except you cannot use the ASP.NET Web Helpers Library.</span></span>


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a><span data-ttu-id="ae975-117">Přidání kódu spuštění webové stránky pro webové stránky ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ae975-117">Adding Website Startup Code for ASP.NET Web Pages</span></span>

<span data-ttu-id="ae975-118">Pro většinu kód, který zapisuje na webových stránkách ASP.NET jednotlivé stránky může obsahovat všechny kód, který je požadovaný pro tuto stránku.</span><span class="sxs-lookup"><span data-stu-id="ae975-118">For much of the code that you write in ASP.NET Web Pages, an individual page can contain all the code that's required for that page.</span></span> <span data-ttu-id="ae975-119">Například pokud stránka odešle e-mailovou zprávu, je možné uvést všechny kód pro tuto operaci na jedné stránce.</span><span class="sxs-lookup"><span data-stu-id="ae975-119">For example, if a page sends an email message, it's possible to put all the code for that operation in a single page.</span></span> <span data-ttu-id="ae975-120">To může zahrnovat kód pro inicializaci nastavení pro odesílání e-mailu (který je pro SMTP server) a odesílání e-mailové zprávy.</span><span class="sxs-lookup"><span data-stu-id="ae975-120">This can include the code to initialize the settings for sending email (that is, for the SMTP server) and for sending the email message.</span></span>

<span data-ttu-id="ae975-121">Ale v některých případech můžete chtít spouštět nějaký kód před spuštěním libovolné stránky na webu.</span><span class="sxs-lookup"><span data-stu-id="ae975-121">However, in some situations, you might want to run some code before any page on the site runs.</span></span> <span data-ttu-id="ae975-122">To je užitečné pro nastavení hodnot, které lze použít kdekoli v síti (označované jako *globální hodnoty*.) Například některé pomocné rutiny vyžadují, abyste zadejte hodnoty jako nastavení e-mailu nebo klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="ae975-122">This is useful for setting values that can be used anywhere in the site (referred to as *global values*.) For example, some helpers require you to provide values like email settings or account keys.</span></span> <span data-ttu-id="ae975-123">Může být užitečný zachovat tato nastavení v globální hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ae975-123">It can be handy to keep these settings in global values.</span></span>

<span data-ttu-id="ae975-124">To provedete tak, že vytvoříte stránku s názvem  *\_AppStart.cshtml* v kořenovém adresáři serveru.</span><span class="sxs-lookup"><span data-stu-id="ae975-124">You can do this by creating a page named *\_AppStart.cshtml* in the root of the site.</span></span> <span data-ttu-id="ae975-125">Pokud tuto stránku existuje, spustí se při prvním požadavku na libovolnou stránku v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="ae975-125">If this page exists, it runs the first time any page in the site is requested.</span></span> <span data-ttu-id="ae975-126">Proto je vhodná k spuštění kódu nastavit globální hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ae975-126">Therefore, it's a good place to run code to set global values.</span></span> <span data-ttu-id="ae975-127">(Protože  *\_AppStart.cshtml* má předponu podtržítko, ASP.NET nepošle stránku v prohlížeči i, pokud uživatelé požadují ji přímo.)</span><span class="sxs-lookup"><span data-stu-id="ae975-127">(Because *\_AppStart.cshtml* has an underscore prefix, ASP.NET won't send the page to a browser even if users request it directly.)</span></span>

<span data-ttu-id="ae975-128">Následující obrázek ukazuje, jak  *\_AppStart.cshtml* stránky funguje.</span><span class="sxs-lookup"><span data-stu-id="ae975-128">The following diagram shows how the *\_AppStart.cshtml* page works.</span></span> <span data-ttu-id="ae975-129">Když přijde žádost pro stránku, a pokud se jedná o první požadavek pro všechny stránky v lokalitě, ASP.NET první kontroly jestli  *\_AppStart.cshtml* stránka existuje.</span><span class="sxs-lookup"><span data-stu-id="ae975-129">When a request comes in for a page, and if this is the first request for any page in the site, ASP.NET first checks whether a *\_AppStart.cshtml* page exists.</span></span> <span data-ttu-id="ae975-130">Pokud ano, některý kód na  *\_AppStart.cshtml* stránka běží, a poté spustí k požadované stránce.</span><span class="sxs-lookup"><span data-stu-id="ae975-130">If so, any code in the *\_AppStart.cshtml* page runs, and then the requested page runs.</span></span>

![[Obrázek]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a><span data-ttu-id="ae975-132">Globální hodnoty nastavení pro svůj web</span><span class="sxs-lookup"><span data-stu-id="ae975-132">Setting Global Values for Your Website</span></span>

1. <span data-ttu-id="ae975-133">V kořenové složce webu služby WebMatrix, vytvořte soubor s názvem  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-133">In the root folder of a WebMatrix website, create a file named *\_AppStart.cshtml*.</span></span> <span data-ttu-id="ae975-134">Soubor musí být v kořenovém adresáři serveru.</span><span class="sxs-lookup"><span data-stu-id="ae975-134">The file must be in the root of the site.</span></span>
2. <span data-ttu-id="ae975-135">Nahradí existující obsah s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="ae975-135">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    <span data-ttu-id="ae975-136">Tento kód ukládá hodnotu v `AppState` slovník, který je automaticky dostupný pro všechny stránky v lokalitě.</span><span class="sxs-lookup"><span data-stu-id="ae975-136">This code stores a value in the `AppState` dictionary, which is automatically available to all pages in the site.</span></span> <span data-ttu-id="ae975-137">Všimněte si, že  *\_AppStart.cshtml* soubor nemá žádné značky v ní.</span><span class="sxs-lookup"><span data-stu-id="ae975-137">Notice that the *\_AppStart.cshtml* file does not have any markup in it.</span></span> <span data-ttu-id="ae975-138">Spustí kód a pak přesměruje na stránku, která původně požádal stránky.</span><span class="sxs-lookup"><span data-stu-id="ae975-138">The page will run the code and then redirect to the page that was originally requested.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ae975-139">Buďte opatrní při chápat kódu  *\_AppStart.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="ae975-139">Be careful when you put code in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="ae975-140">Pokud dojde k chybám v kódu v  *\_AppStart.cshtml* souboru na webu se nespustí.</span><span class="sxs-lookup"><span data-stu-id="ae975-140">If any errors occur in code in the *\_AppStart.cshtml* file, the website won't start.</span></span>
3. <span data-ttu-id="ae975-141">V kořenové složce vytvořte novou stránku s názvem *AppName.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-141">In the root folder, create a new page named *AppName.cshtml*.</span></span>
4. <span data-ttu-id="ae975-142">Výchozí značek a kódu nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ae975-142">Replace the default markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    <span data-ttu-id="ae975-143">Tento kód extrahuje hodnotu z `AppState` objekt, který nastavíte v  *\_AppStart.cshtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="ae975-143">This code extracts the value from the `AppState` object that you set in the *\_AppStart.cshtml* page.</span></span>
5. <span data-ttu-id="ae975-144">Spustit *AppName.cshtml* stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ae975-144">Run the *AppName.cshtml* page in a browser.</span></span> <span data-ttu-id="ae975-145">(Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Na stránce zobrazuje globální hodnota.</span><span class="sxs-lookup"><span data-stu-id="ae975-145">(Make sure the page is selected in the **Files** workspace before you run it.) The page displays the global value.</span></span> 

    ![[Obrázek]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a><span data-ttu-id="ae975-147">Nastavení hodnot pro pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="ae975-147">Setting Values for Helpers</span></span>

<span data-ttu-id="ae975-148">Vhodné využít pro  *\_AppStart.cshtml* soubor je nastavit hodnoty pro pomocné rutiny, které používáte ve vaší lokalitě a které je nutné inicializovat.</span><span class="sxs-lookup"><span data-stu-id="ae975-148">A good use for the *\_AppStart.cshtml* file is to set values for helpers that you use in your site and that have to be initialized.</span></span> <span data-ttu-id="ae975-149">Typickým příkladem jsou nastavení e-mailu pro `WebMail` pomocné rutiny a privátní a veřejné klíče pro `ReCaptcha` pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="ae975-149">Typical examples are email settings for the `WebMail` helper and the private and public keys for the `ReCaptcha` helper.</span></span> <span data-ttu-id="ae975-150">V takových případech můžete nastavit hodnoty jednou  *\_AppStart.cshtml* a pak je již nastavena pro všechny stránky ve vaší lokalitě.</span><span class="sxs-lookup"><span data-stu-id="ae975-150">In cases like these, you can set the values once in the *\_AppStart.cshtml* and then they're already set for all the pages in your site.</span></span>

<span data-ttu-id="ae975-151">Tento postup ukazuje, jak nastavit `WebMail` nastavení globálně.</span><span class="sxs-lookup"><span data-stu-id="ae975-151">This procedure shows you how to set `WebMail` settings globally.</span></span> <span data-ttu-id="ae975-152">(Další informace o používání `WebMail` pomocné rutiny, najdete v části [přidání e-mailu na web ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)</span><span class="sxs-lookup"><span data-stu-id="ae975-152">(For more information about using the `WebMail` helper, see [Adding Email to an ASP.NET Web Pages Site](../getting-started/11-adding-email-to-your-web-site.md).)</span></span>

1. <span data-ttu-id="ae975-153">Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste to ještě už přidali.</span><span class="sxs-lookup"><span data-stu-id="ae975-153">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="ae975-154">Pokud ještě nemáte  *\_AppStart.cshtml* souboru, v kořenové složce webu, vytvořte soubor s názvem  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-154">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
3. <span data-ttu-id="ae975-155">Přidejte následující `WebMail` nastavení  *\_AppStart.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="ae975-155">Add the following `WebMail` settings to the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    <span data-ttu-id="ae975-156">Upravit následující související nastavení v kódu e-mailu:</span><span class="sxs-lookup"><span data-stu-id="ae975-156">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="ae975-157">Nastavit `your-SMTP-host` na název serveru SMTP, který máte přístup.</span><span class="sxs-lookup"><span data-stu-id="ae975-157">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="ae975-158">Nastavit `your-user-name-here` na uživatelské jméno pro svůj účet serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="ae975-158">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="ae975-159">Nastavit `your-account-password` na heslo pro svůj účet serveru SMTP.</span><span class="sxs-lookup"><span data-stu-id="ae975-159">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="ae975-160">Nastavit `your-email-address-here` vlastní e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="ae975-160">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="ae975-161">Toto je e-mailovou adresu, které je zpráva odeslána z.</span><span class="sxs-lookup"><span data-stu-id="ae975-161">This is the email address that the message is sent from.</span></span> <span data-ttu-id="ae975-162">(Někteří poskytovatelé e-mailu Nenechte si můžete nastavit jinou `From` adres a bude používat vaše uživatelské jméno jako `From` adresu.)</span><span class="sxs-lookup"><span data-stu-id="ae975-162">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     <span data-ttu-id="ae975-163">Další informace o nastavení SMTP, najdete v části [konfigurace nastavení e-mailu](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) v článku [odesílání e-mailu z webových stránek ASP.NET (Razor) stránky](https://go.microsoft.com/fwlink/?LinkID=202899) a [problémy s odesílání e-mailu](https://go.microsoft.com/fwlink/?LinkId=253001#email)v [ASP.NET Web Pages Průvodce řešením potíží (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).</span><span class="sxs-lookup"><span data-stu-id="ae975-163">For more information about SMTP settings, see [Configuring Email Settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) in the article [Sending Email from an ASP.NET Web Pages (Razor) Site](https://go.microsoft.com/fwlink/?LinkID=202899) and [Issues with Sending Email](https://go.microsoft.com/fwlink/?LinkId=253001#email) in the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>
4. <span data-ttu-id="ae975-164">Uložit  *\_AppStart.cshtml* souboru a zavřete ho.</span><span class="sxs-lookup"><span data-stu-id="ae975-164">Save the *\_AppStart.cshtml* file and close it.</span></span>
5. <span data-ttu-id="ae975-165">V kořenové složce webu, vytvořte novou stránku s názvem *TestEmail.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-165">In the root folder of a website, create new page named *TestEmail.cshtml*.</span></span>
6. <span data-ttu-id="ae975-166">Nahradí existující obsah s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="ae975-166">Replace the existing content with the following:</span></span> 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. <span data-ttu-id="ae975-167">Spustit *TestEmail.cshtml* stránku v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ae975-167">Run the *TestEmail.cshtml* page in a browser.</span></span>
8. <span data-ttu-id="ae975-168">Vyplňte pole odeslat sami e-mailovou zprávu a pak klikněte na **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="ae975-168">Fill in the fields to send yourself an email message and then click **Send**.</span></span>
9. <span data-ttu-id="ae975-169">Zkontrolujte e-mailu a ujistěte se, že jste, že jste podmínky zprávy.</span><span class="sxs-lookup"><span data-stu-id="ae975-169">Check your email to make sure you've gotten the message.</span></span>

<span data-ttu-id="ae975-170">Důležitou součástí v tomto příkladu je, že nastavení, která obvykle neměnit – jako název serveru SMTP a přihlašovacích údajů e-mailu – jsou nastavené  *\_AppStart.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="ae975-170">The important part of this example is that the settings that you don't usually change — like the name of your SMTP server and your email credentials — are set in the *\_AppStart.cshtml* file.</span></span> <span data-ttu-id="ae975-171">Tímto způsobem, nemusíte je znovu nastavit v každé stránce, kde odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ae975-171">That way you don't need to set them again in each page where you send email.</span></span> <span data-ttu-id="ae975-172">(I když Pokud z nějakého důvodu potřebujete změnit tato nastavení, můžete nastavit je samostatně na stránce.) Na stránce můžete nastavit pouze hodnoty, které obvykle změnit pokaždé, když, jako je příjemce a tělo e-mailové zprávy.</span><span class="sxs-lookup"><span data-stu-id="ae975-172">(Although if for some reason you need to change those settings, you can set them individually in a page.) In the page, you only set the values that typically change each time, like the recipient and the body of the email message.</span></span>

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a><span data-ttu-id="ae975-173">Spuštěním kódu před a po souborů ve složce</span><span class="sxs-lookup"><span data-stu-id="ae975-173">Running Code Before and After Files in a Folder</span></span>

<span data-ttu-id="ae975-174">Stejně jako můžete použít  *\_AppStart.cshtml* napsat kód, před spuštěním stránky v lokalitě, můžete napsat kód, který běží před (a po) všechny stránky v určité složce spustit.</span><span class="sxs-lookup"><span data-stu-id="ae975-174">Just like you can use *\_AppStart.cshtml* to write code before pages in the site run, you can write code that runs before (and after) any page in a particular folder run.</span></span> <span data-ttu-id="ae975-175">To je užitečné pro takové věci, jako nastavení stejné stránce rozložení pro všechny stránky ve složce, nebo pro kontrolu, uživatel je přihlášen před spuštěním na stránce ve složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-175">This is useful for things like setting the same layout page for all the pages in a folder, or for checking that a user is logged in before running a page in the folder.</span></span>

<span data-ttu-id="ae975-176">Pro stránky na konkrétní složky, můžete vytvořit kód v souboru s názvem  *\_PageStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-176">For pages in particular folders, you can create code in a file named *\_PageStart.cshtml*.</span></span> <span data-ttu-id="ae975-177">Následující obrázek ukazuje, jak  *\_PageStart.cshtml* stránky funguje.</span><span class="sxs-lookup"><span data-stu-id="ae975-177">The following diagram shows how the *\_PageStart.cshtml* page works.</span></span> <span data-ttu-id="ae975-178">Když přijde žádost pro stránku, ASP.NET nejdřív zkontroluje  *\_AppStart.cshtml* stránky a který je spuštěn.</span><span class="sxs-lookup"><span data-stu-id="ae975-178">When a request comes in for a page, ASP.NET first checks for a *\_AppStart.cshtml* page and runs that.</span></span> <span data-ttu-id="ae975-179">Pak ASP.NET zkontroluje, jestli je  *\_PageStart.cshtml* stránky, a pokud ano, který spouští.</span><span class="sxs-lookup"><span data-stu-id="ae975-179">Then ASP.NET checks whether there's a *\_PageStart.cshtml* page, and if so, runs that.</span></span> <span data-ttu-id="ae975-180">Pak spustí k požadované stránce.</span><span class="sxs-lookup"><span data-stu-id="ae975-180">It then runs the requested page.</span></span>

<span data-ttu-id="ae975-181">Uvnitř  *\_PageStart.cshtml* stránky, můžete určit, kam během zpracování, které chcete k požadované stránce spuštění zahrnutím `RunPage` metoda.</span><span class="sxs-lookup"><span data-stu-id="ae975-181">Inside the *\_PageStart.cshtml* page, you can specify where during processing you want the requested page to run by including a `RunPage` method.</span></span> <span data-ttu-id="ae975-182">To vám umožní spustit kód před spuštěním k požadované stránce a potom znovu za ním.</span><span class="sxs-lookup"><span data-stu-id="ae975-182">This lets you run code before the requested page runs and then again after it.</span></span> <span data-ttu-id="ae975-183">Pokud nechcete zahrnout `RunPage`, kód v  *\_PageStart.cshtml* běží a potom k požadované stránce automaticky spustí.</span><span class="sxs-lookup"><span data-stu-id="ae975-183">If you don't include `RunPage`, all the code in *\_PageStart.cshtml* runs, and then the requested page runs automatically.</span></span>

![[Obrázek]](18-customizing-site-wide-behavior/_static/image3.jpg)

<span data-ttu-id="ae975-185">Technologie ASP.NET umožňuje vytvořit hierarchii  *\_PageStart.cshtml* soubory.</span><span class="sxs-lookup"><span data-stu-id="ae975-185">ASP.NET lets you create a hierarchy of *\_PageStart.cshtml* files.</span></span> <span data-ttu-id="ae975-186">Můžete vložit  *\_PageStart.cshtml* soubor v kořenovém adresáři serveru a všechny podsložky.</span><span class="sxs-lookup"><span data-stu-id="ae975-186">You can put a *\_PageStart.cshtml* file in the root of the site and in any subfolder.</span></span> <span data-ttu-id="ae975-187">Při požadavku na stránku  *\_PageStart.cshtml* souboru na nejvyšší úrovni (který je nejblíže kořenovému adresáři webu) spustí, za nímž následuje  *\_PageStart.cshtml* souboru v dalším podsložku, a tak dále dolů strukturu podsložky, dokud požadavek dosáhne složku, která obsahuje požadovanou stránku.</span><span class="sxs-lookup"><span data-stu-id="ae975-187">When a page is requested, the *\_PageStart.cshtml* file at the top-most level (nearest to the site root) runs, followed by the *\_PageStart.cshtml* file in the next subfolder, and so on down the subfolder structure until the request reaches the folder that contains the requested page.</span></span> <span data-ttu-id="ae975-188">Po všech příslušných  *\_PageStart.cshtml* spustili soubory, spustí se požadovaná stránka.</span><span class="sxs-lookup"><span data-stu-id="ae975-188">After all the applicable *\_PageStart.cshtml* files have run, the requested page runs.</span></span>

<span data-ttu-id="ae975-189">Například můžete mít následující kombinace  *\_PageStart.cshtml* soubory a *Default.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="ae975-189">For example, you might have the following combination of *\_PageStart.cshtml* files and *Default.cshtml* file:</span></span>

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

<span data-ttu-id="ae975-190">Při spuštění */myfolder/default.cshtml*, zobrazí se následující:</span><span class="sxs-lookup"><span data-stu-id="ae975-190">When you run */myfolder/default.cshtml*, you'll see the following:</span></span>

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a><span data-ttu-id="ae975-191">Spuštění kódu inicializace pro všechny stránky ve složce</span><span class="sxs-lookup"><span data-stu-id="ae975-191">Running Initialization Code for All Pages in a Folder</span></span>

<span data-ttu-id="ae975-192">Vhodné využít pro  *\_PageStart.cshtml* souborů je k chybě při inicializaci stránce rozložení pro všechny soubory v jedné složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-192">A good use for *\_PageStart.cshtml* files is to initialize the same layout page for all files in a single folder.</span></span>

1. <span data-ttu-id="ae975-193">V kořenové složce vytvořte novou složku s názvem *InitPages*.</span><span class="sxs-lookup"><span data-stu-id="ae975-193">In the root folder, create a new folder named *InitPages*.</span></span>
2. <span data-ttu-id="ae975-194">V *InitPages* složku vašeho webu, vytvořte soubor s názvem  *\_PageStart.cshtml* a výchozí značek a kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-194">In the *InitPages* folder of your website, create a file named *\_PageStart.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. <span data-ttu-id="ae975-195">V kořenovém adresáři webu vytvořte složku s názvem *sdílené*.</span><span class="sxs-lookup"><span data-stu-id="ae975-195">In the root of the website, create a folder named *Shared*.</span></span>
4. <span data-ttu-id="ae975-196">V *sdílené* složky, vytvořte soubor s názvem  *\_Layout1.cshtml* a výchozí značek a kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-196">In the *Shared* folder, create a file named *\_Layout1.cshtml* and replace the default markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. <span data-ttu-id="ae975-197">V *InitPages* složky, vytvořte soubor s názvem *Content1.cshtml* a nahradí existující obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-197">In the *InitPages* folder, create a file named *Content1.cshtml* and replace the existing content with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. <span data-ttu-id="ae975-198">V *InitPages* složce vytvořte jiný soubor s názvem *Content2.cshtml* a výchozí značka nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-198">In the *InitPages* folder, create another file named *Content2.cshtml* and replace the default markup with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. <span data-ttu-id="ae975-199">Spustit *Content1.cshtml* v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ae975-199">Run *Content1.cshtml* in a browser.</span></span> 

    ![[Obrázek]](18-customizing-site-wide-behavior/_static/image4.jpg)

    <span data-ttu-id="ae975-201">Při *Content1.cshtml* stránka běží,  *\_PageStart.cshtml* souboru sady `Layout` a také nastaví `PageData["MyBackground"]` na barvu.</span><span class="sxs-lookup"><span data-stu-id="ae975-201">When the *Content1.cshtml* page runs, the *\_PageStart.cshtml* file sets `Layout` and also sets `PageData["MyBackground"]` to a color.</span></span> <span data-ttu-id="ae975-202">V *Content1.cshtml*, se použijí rozložení a barvy.</span><span class="sxs-lookup"><span data-stu-id="ae975-202">In *Content1.cshtml*, the layout and color are applied.</span></span>
8. <span data-ttu-id="ae975-203">Zobrazení *Content2.cshtml* v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ae975-203">Display *Content2.cshtml* in a browser.</span></span> 

    <span data-ttu-id="ae975-204">Rozložení je stejné, protože obě stránky použít stejnou rozložení stránky a barvu jako v inicializovat  *\_PageStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-204">The layout is the same, because both pages use the same layout page and color as initialized in *\_PageStart.cshtml*.</span></span>

## <a name="using-pagestartcshtml-to-handle-errors"></a><span data-ttu-id="ae975-205">Pomocí \_PageStart.cshtml ke zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="ae975-205">Using \_PageStart.cshtml to Handle Errors</span></span>

<span data-ttu-id="ae975-206">Jiné dobré použít pro  *\_PageStart.cshtml* souboru je vytvoření způsob, jak zpracovávat programovací chyby (výjimky), které můžou nastat v některém *.cshtml* stránku ve složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-206">Another good use for the *\_PageStart.cshtml* file is to create a way to handle programming errors (exceptions) that might occur in any *.cshtml* page in a folder.</span></span> <span data-ttu-id="ae975-207">Tento příklad ukazuje jeden způsob, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="ae975-207">This example shows you one way to do this.</span></span>

1. <span data-ttu-id="ae975-208">V kořenové složce vytvořte složku s názvem *InitCatch*.</span><span class="sxs-lookup"><span data-stu-id="ae975-208">In the root folder, create a folder named *InitCatch*.</span></span>
2. <span data-ttu-id="ae975-209">V *InitCatch* složku vašeho webu, vytvořte soubor s názvem  *\_PageStart.cshtml* a existující značek a kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-209">In the *InitCatch* folder of your website, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    <span data-ttu-id="ae975-210">V tomto kódu, můžete se pokusit spustit k požadované stránce explicitně voláním `RunPage` metodu `try` bloku.</span><span class="sxs-lookup"><span data-stu-id="ae975-210">In this code, you try running the requested page explicitly by calling the `RunPage` method inside a `try` block.</span></span> <span data-ttu-id="ae975-211">Pokud dojde k chybám programování v požadované stránce, kód uvnitř `catch` blokovat spuštění.</span><span class="sxs-lookup"><span data-stu-id="ae975-211">If any programming errors occur in the requested page, the code inside the `catch` block runs.</span></span> <span data-ttu-id="ae975-212">V tomto případě kód provede přesměrování na stránku (*Error.cshtml*) a předá název souboru, který jako část adresy URL došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="ae975-212">In this case, the code redirects to a page (*Error.cshtml*) and passes the name of the file that experienced the error as part of the URL.</span></span> <span data-ttu-id="ae975-213">(Vytvoříte stránky za chvíli.)</span><span class="sxs-lookup"><span data-stu-id="ae975-213">(You'll create the page shortly.)</span></span>
3. <span data-ttu-id="ae975-214">V *InitCatch* složku vašeho webu, vytvořte soubor s názvem *Exception.cshtml* a existující značek a kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-214">In the *InitCatch* folder of your website, create a file named *Exception.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    <span data-ttu-id="ae975-215">Pro účely tohoto příkladu jaké úlohy na této stránce úmyslně vytváří chybu tak, že zkusíte otevřít databázový soubor, který neexistuje.</span><span class="sxs-lookup"><span data-stu-id="ae975-215">For purposes of this example, what you're doing in this page is deliberately creating an error by trying to open a database file that doesn't exist.</span></span>
4. <span data-ttu-id="ae975-216">V kořenové složce vytvořte soubor s názvem *Error.cshtml* a existující značek a kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-216">In the root folder, create a file named *Error.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    <span data-ttu-id="ae975-217">Na této stránce výraz `@Request["source"]` získá hodnotu z adresy URL a zobrazí se.</span><span class="sxs-lookup"><span data-stu-id="ae975-217">In this page, the expression `@Request["source"]` gets the value out of the URL and displays it.</span></span>
5. <span data-ttu-id="ae975-218">Na panelu nástrojů klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ae975-218">In the toolbar, click **Save**.</span></span>
6. <span data-ttu-id="ae975-219">Spustit *Exception.cshtml* v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ae975-219">Run *Exception.cshtml* in a browser.</span></span> 

    ![[Obrázek]](18-customizing-site-wide-behavior/_static/image5.jpg)

    <span data-ttu-id="ae975-221">Vzhledem k tomu, že dojde k chybě v *Exception.cshtml*,  *\_PageStart.cshtml* k přesměrování stránky *Error.cshtml* souboru, který zobrazí zprávu.</span><span class="sxs-lookup"><span data-stu-id="ae975-221">Because an error occurs in *Exception.cshtml*, the *\_PageStart.cshtml* page redirects to the *Error.cshtml* file, which displays the message.</span></span>

    <span data-ttu-id="ae975-222">Další informace o výjimkách najdete v tématu [Úvod k technologii ASP.NET Web Pages programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=251587).</span><span class="sxs-lookup"><span data-stu-id="ae975-222">For more information about exceptions, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).</span></span>

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a><span data-ttu-id="ae975-223">Pomocí \_PageStart.cshtml omezit přístup ke složce</span><span class="sxs-lookup"><span data-stu-id="ae975-223">Using \_PageStart.cshtml to Restrict Folder Access</span></span>

<span data-ttu-id="ae975-224">Můžete také  *\_PageStart.cshtml* souboru omezit přístup na všechny soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-224">You can also use the *\_PageStart.cshtml* file to restrict access to all the files in a folder.</span></span>

1. <span data-ttu-id="ae975-225">Ve službě WebMatrix, vytvořte nový web pomocí **šablonu z webu** možnost.</span><span class="sxs-lookup"><span data-stu-id="ae975-225">In WebMatrix, create a new website using the **Site From Template** option.</span></span>
2. <span data-ttu-id="ae975-226">Vyberte z dostupných šablon, **Starter Site**.</span><span class="sxs-lookup"><span data-stu-id="ae975-226">From the available templates, select **Starter Site**.</span></span>
3. <span data-ttu-id="ae975-227">V kořenové složce vytvořte složku s názvem *AuthenticatedContent*.</span><span class="sxs-lookup"><span data-stu-id="ae975-227">In the root folder, create a folder named *AuthenticatedContent*.</span></span>
4. <span data-ttu-id="ae975-228">V *AuthenticatedContent* složky, vytvořte soubor s názvem  *\_PageStart.cshtml* a existující značek a kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="ae975-228">In the *AuthenticatedContent* folder, create a file named *\_PageStart.cshtml* and replace the existing markup and code with the following:</span></span> 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    <span data-ttu-id="ae975-229">Kód spustí tím, že všechny soubory ve složce mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="ae975-229">The code starts by preventing all files in the folder from being cached.</span></span> <span data-ttu-id="ae975-230">(To je nutný v případech, například veřejné počítače, kde nechcete, aby jeden uživatel stránky uložené v mezipaměti k dispozici pro další uživatele.) V dalším kroku kód určuje, zda uživatel přihlásí na web před zobrazením kterákoli ze stránek ve složce.</span><span class="sxs-lookup"><span data-stu-id="ae975-230">(This is required for scenarios like public computers, where you don't want one user's cached pages to be available to the next user.) Next, the code determines whether the user has signed in to the site before they can view any of the pages in the folder.</span></span> <span data-ttu-id="ae975-231">Pokud se uživatel není přihlášen, kód přesměruje na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="ae975-231">If the user is not signed in, the code redirects to the login page.</span></span> <span data-ttu-id="ae975-232">Přihlašovací stránky může vrátit uživatele na stránku, která původně požádal Pokud zahrnete hodnotu řetězce dotazu s názvem `ReturnUrl`.</span><span class="sxs-lookup"><span data-stu-id="ae975-232">The login page can return the user to the page that was originally requested if you include a query string value named `ReturnUrl`.</span></span>
5. <span data-ttu-id="ae975-233">Vytvořit novou stránku v *AuthenticatedContent* složku s názvem *Page.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ae975-233">Create a new page in the *AuthenticatedContent* folder named *Page.cshtml*.</span></span>
6. <span data-ttu-id="ae975-234">Nahraďte kód výchozí s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="ae975-234">Replace the default markup with the following:</span></span>  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. <span data-ttu-id="ae975-235">Spustit *Page.cshtml* v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="ae975-235">Run *Page.cshtml* in a browser.</span></span> <span data-ttu-id="ae975-236">Kód vás přesměruje na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="ae975-236">The code redirects you to a login page.</span></span> <span data-ttu-id="ae975-237">Je nutné zaregistrovat před přihlášení.</span><span class="sxs-lookup"><span data-stu-id="ae975-237">You must register before logging in.</span></span> <span data-ttu-id="ae975-238">Po zaregistrované a přihlášení, můžete přejít na stránku a zobrazte její obsah.</span><span class="sxs-lookup"><span data-stu-id="ae975-238">After you've registered and logged in, you can navigate to the page and view its contents.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="ae975-239">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="ae975-239">Additional Resources</span></span>

[<span data-ttu-id="ae975-240">Úvod do programování pomocí syntaxe Razor rozhraní ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="ae975-240">Introduction to ASP.NET Web Pages Programming Using the Razor Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkID=251587)
