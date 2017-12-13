---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: "Přihlášení pomocí externí servery v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Tento článek vysvětluje, jak se přihlásit na váš web ASP.NET Web Pages (Razor) pomocí služby Facebook, Google, Twitter, Yahoo a jiných lokalitách – to znamená, jak podporovat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="c066c-103">Přihlášení pomocí externí servery v Web Pages (Razor) technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c066c-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="c066c-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c066c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c066c-105">Tento článek vysvětluje, jak se přihlásit na váš web ASP.NET Web Pages (Razor) pomocí služby Facebook, Google, Twitter, Yahoo a jiných lokalitách – to znamená, jak ve vaší lokalitě podporovat OAuth a OpenID.</span><span class="sxs-lookup"><span data-stu-id="c066c-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="c066c-106">Získáte informace:</span><span class="sxs-lookup"><span data-stu-id="c066c-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="c066c-107">Postup povolení přihlášení z jiných webů, když použijete šablonu Starter web služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c066c-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="c066c-108">Toto je funkce technologie ASP.NET byla zavedená v článku:</span><span class="sxs-lookup"><span data-stu-id="c066c-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="c066c-109">`OAuthWebSecurity` Pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="c066c-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c066c-110">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="c066c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c066c-111">Rozhraní ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="c066c-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="c066c-112">Služba WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="c066c-112">WebMatrix 3</span></span>

<span data-ttu-id="c066c-113">Zahrnuje podporu pro ASP.NET Web Pages [OAuth](http://oauth.net/) a [OpenID](http://openid.net/) zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="c066c-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="c066c-114">Pomocí těchto poskytovatelů, můžete je nechat protokolu uživatele na váš web pomocí svých existujících přihlašovacích údajů ze sítě Facebook, Twitter, Microsoft a Google.</span><span class="sxs-lookup"><span data-stu-id="c066c-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="c066c-115">K přihlášení pomocí účtu sítě Facebook, například uživatelé mohou zvolit právě Facebook ikonu, která je přesměruje na přihlašovací stránku služby Facebook, kde zadat informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="c066c-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="c066c-116">Přihlášení k síti Facebook se pak můžete přidružit ke svému účtu na váš web.</span><span class="sxs-lookup"><span data-stu-id="c066c-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="c066c-117">Související vylepšení funkcí členství webové stránky je, že uživatelé mohou přidružit více přihlášení (včetně přihlášení ze sociálních sítí) s jeden účet na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="c066c-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="c066c-118">Tento obrázek zobrazuje stránku pro přihlášení z **Starter Site** šablony, které může uživatel Facebook, Twitter, Google nebo Microsoft ikonu povolíte protokolování s externím účtu:</span><span class="sxs-lookup"><span data-stu-id="c066c-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![externí poskytovatelé](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="c066c-120">Můžete povolit OAuth a OpenID členství uncommenting po zadání několika řádků kódu **Starter Site** šablony.</span><span class="sxs-lookup"><span data-stu-id="c066c-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="c066c-121">Metody a vlastnosti, použijete pro práci s OAuth a OpenID poskytovatelé jsou v `WebMatrix.Security.OAuthWebSecurity` třídy.</span><span class="sxs-lookup"><span data-stu-id="c066c-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="c066c-122">**Starter Site** Šablona zahrnuje úplné členství infrastruktury, s přihlašovací stránky, databáze členství a všechny kód, je třeba dát protokolu uživatele na váš web pomocí přihlašovacích údajů místního nebo ty z jiné lokality .</span><span class="sxs-lookup"><span data-stu-id="c066c-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="c066c-123">Tato část poskytuje příklad, jak umožnit uživatelům přihlášení z externí servery k lokalitě, která je založena na **Starter Site** šablony.</span><span class="sxs-lookup"><span data-stu-id="c066c-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="c066c-124">Po vytvoření úvodní lokality, proveďte tento (podrobnosti použijte):</span><span class="sxs-lookup"><span data-stu-id="c066c-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="c066c-125">Weby, které používají poskytovatele OAuth (Facebook, Twitter a Microsoft) vytvoříte aplikaci na webu externího.</span><span class="sxs-lookup"><span data-stu-id="c066c-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="c066c-126">To vám dává klíče aplikace, které budete potřebovat k vyvolání funkce přihlášení pro tyto lokality.</span><span class="sxs-lookup"><span data-stu-id="c066c-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="c066c-127">Weby, které používají poskytovatele OpenID (Google) není nutné k vytvoření aplikace.</span><span class="sxs-lookup"><span data-stu-id="c066c-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="c066c-128">Pro všechny tyto servery musí mít účet k přihlášení a vytvořit vývojář aplikace.</span><span class="sxs-lookup"><span data-stu-id="c066c-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c066c-129">Aplikace Microsoft přijímat pouze za provozu adresu URL pro funkčního webu, tak pro testování přihlášení nemůžete použít adresu URL místního webu.</span><span class="sxs-lookup"><span data-stu-id="c066c-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="c066c-130">Chcete-li zadat poskytovatele příslušný ověřovací upravit několik souborů ve vašem webu a odeslat přihlášení k webu, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="c066c-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="c066c-131">Tento článek obsahuje samostatné pokyny pro následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="c066c-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="c066c-132">Povolení přihlášení Google</span><span class="sxs-lookup"><span data-stu-id="c066c-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="c066c-133">Povolení přihlášení Facebook</span><span class="sxs-lookup"><span data-stu-id="c066c-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="c066c-134">Povolení přihlášení služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="c066c-135">Povolení přihlášení Google</span><span class="sxs-lookup"><span data-stu-id="c066c-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="c066c-136">Vytvořit nebo otevřít webové stránky ASP.NET Web založený na šabloně Starter web služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c066c-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="c066c-137">Otevřete  *\_AppStart.cshtml* stránky a zrušte komentář u následujícího řádku kódu.</span><span class="sxs-lookup"><span data-stu-id="c066c-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="c066c-138">Testování Google přihlášení</span><span class="sxs-lookup"><span data-stu-id="c066c-138">Testing Google login</span></span>

1. <span data-ttu-id="c066c-139">Spustit *default.cshtml* stránku vašeho webu a zvolte **přihlásit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="c066c-140">Na *přihlášení* stránky v **přihlásit pomocí jiné služby** vyberte buď **Google** nebo **Yahoo** tlačítko Odeslat.</span><span class="sxs-lookup"><span data-stu-id="c066c-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="c066c-141">Tento příklad používá přihlášení Google.</span><span class="sxs-lookup"><span data-stu-id="c066c-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="c066c-142">Webová stránka přesměruje požadavek na přihlašovací stránku Google.</span><span class="sxs-lookup"><span data-stu-id="c066c-142">The web page redirects the request to the Google login page.</span></span>

    ![Google přihlášení](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="c066c-144">Zadejte přihlašovací údaje pro existující účet Google.</span><span class="sxs-lookup"><span data-stu-id="c066c-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="c066c-145">Pokud Google dotazem, zda chcete povolit *Localhost* používat informace z účtu, klikněte na tlačítko **povolit**.</span><span class="sxs-lookup"><span data-stu-id="c066c-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="c066c-146">Kód používá Google token k ověření uživatele a vrátí na tuto stránku na vašem webu.</span><span class="sxs-lookup"><span data-stu-id="c066c-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="c066c-147">Tato stránka umožňuje uživatelům přidružení jejich Google přihlášení s existujícím účtem na vašem webu, nebo se můžete zaregistrovat nový účet na svém webu přidružit externí přihlášení s.</span><span class="sxs-lookup"><span data-stu-id="c066c-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="c066c-149">Vyberte **přidružit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-149">Choose the **Associate** button.</span></span> <span data-ttu-id="c066c-150">V prohlížeči vrátí na domovskou stránku vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c066c-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="c066c-151">Povolení přihlášení Facebook</span><span class="sxs-lookup"><span data-stu-id="c066c-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="c066c-152">Přejděte na [Facebook vývojáři lokality](https://developers.facebook.com/apps) (protokolu případě, že jste již přihlášeni).</span><span class="sxs-lookup"><span data-stu-id="c066c-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="c066c-153">Vyberte **vytvořit novou aplikaci** tlačítko a pak postupujte podle pokynů k pojmenování a vytvořit novou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c066c-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="c066c-154">V části **vyberte, jak bude vaše aplikace integrovat Facebook**, vyberte **webu** části.</span><span class="sxs-lookup"><span data-stu-id="c066c-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="c066c-155">Vyplňte **adresa URL webu** pole s adresou URL vašeho webu (například `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="c066c-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="c066c-156">**Domény** pole je volitelné; můžete použít k ověření pro celou doménu (například *example.com*).</span><span class="sxs-lookup"><span data-stu-id="c066c-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="c066c-157">Pokud používáte lokalitu v místním počítači s adresou URL jako `http://localhost:12345` (kde číslo je číslem místního portu), přidáním této hodnoty **adresa URL webu** pole pro testování vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="c066c-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="c066c-158">Ale kdykoli se číslo portu změny místní lokality, budete muset aktualizovat **adresa URL webu** pole vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c066c-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="c066c-159">Vyberte **uložit změny** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="c066c-160">Vyberte **aplikace** znovu a poté zobrazte úvodní stránky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c066c-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="c066c-161">Kopírování **ID aplikace** a **tajný klíč aplikace** hodnoty pro vaši aplikaci a vložte je do dočasného textového souboru.</span><span class="sxs-lookup"><span data-stu-id="c066c-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="c066c-162">Tyto hodnoty budou předat poskytovatele sítě Facebook v kódu webové stránky.</span><span class="sxs-lookup"><span data-stu-id="c066c-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="c066c-163">Ukončení webu pro vývojáře Facebook.</span><span class="sxs-lookup"><span data-stu-id="c066c-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="c066c-164">Teď provedete změny dvě stránky ve vašem webu tak, aby uživatelé budou moci přihlásit do lokality pomocí účtů služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="c066c-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="c066c-165">Vytvořit nebo otevřít webové stránky ASP.NET Web založený na šabloně Starter web služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c066c-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="c066c-166">Otevřete  *\_AppStart.cshtml* stránky a zrušte komentář kódu pro zprostředkovatele OAuth pro Facebook.</span><span class="sxs-lookup"><span data-stu-id="c066c-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="c066c-167">Blok uncommented kódu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c066c-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="c066c-168">Kopírování **ID aplikace** hodnotu z aplikace Facebook jako hodnotu `appId` parametr (v uvozovkách).</span><span class="sxs-lookup"><span data-stu-id="c066c-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="c066c-169">Kopírování **tajný klíč aplikace** hodnotu z aplikace Facebook, jako `appSecret` hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="c066c-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="c066c-170">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="c066c-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="c066c-171">Testování Facebook přihlášení</span><span class="sxs-lookup"><span data-stu-id="c066c-171">Testing Facebook login</span></span>

1. <span data-ttu-id="c066c-172">Spuštění tohoto webu *default.cshtml* stránce a vyberte položku **přihlášení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="c066c-173">Na *přihlášení* stránky v **přihlásit pomocí jiné služby** zvolte **Facebook** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c066c-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="c066c-174">Webová stránka přesměruje požadavek na přihlašovací stránku služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="c066c-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="c066c-176">Přihlaste se k účtu sítě Facebook.</span><span class="sxs-lookup"><span data-stu-id="c066c-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="c066c-177">Kód k vašemu ověření používá token služby Facebook a vrátí na stránky, kde je možné přidružit Facebook přihlašovací jméno vašeho webu přihlášení.</span><span class="sxs-lookup"><span data-stu-id="c066c-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="c066c-178">Uživatelského jména nebo e-mailové adresy se naplní do **e-mailu** pole ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="c066c-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="c066c-180">Vyberte **přidružit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="c066c-181">Vrátí prohlížeči na domovskou stránku a jste přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="c066c-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="c066c-182">Povolení přihlášení služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="c066c-183">Vyhledejte [Twitter vývojáři lokality](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="c066c-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="c066c-184">Vyberte **vytvořit aplikaci** propojit a přihlaste se k webu.</span><span class="sxs-lookup"><span data-stu-id="c066c-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="c066c-185">Na **vytvořit aplikaci** formuláři, zadejte **název** a **popis** pole.</span><span class="sxs-lookup"><span data-stu-id="c066c-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="c066c-186">V **webu** pole, zadejte adresu URL vašeho webu (například `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="c066c-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="c066c-187">Pokud testujete váš web místně (pomocí adresy URL jako `http://localhost:12345`), služby Twitter nemusí přijmout adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c066c-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="c066c-188">Nicméně je možné použít místní smyčky IP adresu (například `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="c066c-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="c066c-189">Tato funkce zjednodušuje proces testování aplikace s místně.</span><span class="sxs-lookup"><span data-stu-id="c066c-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="c066c-190">Však pokaždé, když číslo portu místní lokality změní, budete muset aktualizovat **webu** pole vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="c066c-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="c066c-191">V **adresu URL zpětné volání** pole, zadejte adresu URL stránky ve vašem webu, kterou mají uživatelé se vraťte do po přihlášení do služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="c066c-192">Například uživatelé odeslat na domovskou stránku Starter Site (který rozpozná stav jejich přihlášení), zadejte stejnou adresu URL, kterou jste zadali v **webu** pole.</span><span class="sxs-lookup"><span data-stu-id="c066c-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="c066c-193">Přijměte podmínky a vyberte **vytvořit aplikaci služby Twitter** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="c066c-194">Na **Moje aplikace** úvodní stránka, vyberte aplikaci, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c066c-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="c066c-195">Na **podrobnosti** , posuňte se dolů a klikněte na příkaz **vytvořit Moje přístup Token** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="c066c-196">Na **podrobnosti** kartě, zkopírujte **uživatelský klíč** a **uživatelský tajný klíč** hodnoty pro vaši aplikaci a vložte je do dočasného textového souboru.</span><span class="sxs-lookup"><span data-stu-id="c066c-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="c066c-197">Tyto hodnoty budete předáte v kódu webové stránky k poskytovateli služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="c066c-198">Ukončení webu služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-198">Exit the Twitter site.</span></span>

<span data-ttu-id="c066c-199">Nyní můžete měnit dvě stránky ve vašem webu tak, aby uživatelé budou moci přihlásit do lokality pomocí účtů služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="c066c-200">Vytvořit nebo otevřít webové stránky ASP.NET Web založený na šabloně Starter web služby WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="c066c-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="c066c-201">Otevřete  *\_AppStart.cshtml* stránky a zrušte komentář kódu pro zprostředkovatele služby Twitter OAuth.</span><span class="sxs-lookup"><span data-stu-id="c066c-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="c066c-202">Blok uncommented kódu vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="c066c-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="c066c-203">Kopírování **uživatelský klíč** hodnotu z aplikace služby Twitter jako hodnotu `consumerKey` parametr (v uvozovkách).</span><span class="sxs-lookup"><span data-stu-id="c066c-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="c066c-204">Kopírování **uživatelský tajný klíč** hodnotu z aplikace služby Twitter jako hodnotu `consumerSecret` parametr.</span><span class="sxs-lookup"><span data-stu-id="c066c-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="c066c-205">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="c066c-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="c066c-206">Testování přihlášení služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-206">Testing Twitter login</span></span>

1. <span data-ttu-id="c066c-207">Spustit *default.cshtml* stránku vašeho webu a zvolte **přihlášení** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="c066c-208">Na *přihlášení* stránky v **přihlásit pomocí jiné služby** zvolte **Twitter** ikonu.</span><span class="sxs-lookup"><span data-stu-id="c066c-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="c066c-209">Webová stránka přesměruje požadavek na přihlašovací stránku služby Twitter pro aplikaci, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c066c-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="c066c-211">Přihlaste se k účtu sítě Twitter.</span><span class="sxs-lookup"><span data-stu-id="c066c-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="c066c-212">Kód používá token služby Twitter. k ověření uživatele a vrátí můžete na stránky, kde můžete přidružit vaše přihlášení pomocí účtu webu.</span><span class="sxs-lookup"><span data-stu-id="c066c-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="c066c-213">Jméno nebo e-mailovou adresu se naplní do **e-mailu** pole ve formuláři.</span><span class="sxs-lookup"><span data-stu-id="c066c-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="c066c-215">Vyberte **přidružit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="c066c-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="c066c-216">Vrátí prohlížeči na domovskou stránku a jste přihlášeni.</span><span class="sxs-lookup"><span data-stu-id="c066c-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="c066c-217">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="c066c-217">Additional Resources</span></span>


- [<span data-ttu-id="c066c-218">Přizpůsobení chování na webu</span><span class="sxs-lookup"><span data-stu-id="c066c-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="c066c-219">Přidání členství a zabezpečení do stránky webu technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c066c-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
