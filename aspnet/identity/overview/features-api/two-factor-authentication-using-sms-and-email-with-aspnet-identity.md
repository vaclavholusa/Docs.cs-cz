---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity | Microsoft Docs
author: HaoK
description: Tento kurz vám ukáže, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS a e-mailu. Tento článek napsal Rick Anderson ( @RickAndMSFT ), za...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876134"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="aeec9-104">Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="aeec9-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="aeec9-105">podle [Haovi společnosti ani](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="aeec9-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="aeec9-106">Tento kurz vám ukáže, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS a e-mailu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="aeec9-107">Tento článek napsal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), ani Haovi společnosti a Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="aeec9-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="aeec9-108">Ukázka NuGet napsal především ani Haovi společnosti.</span><span class="sxs-lookup"><span data-stu-id="aeec9-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="aeec9-109">Toto téma obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="aeec9-109">This topic covers the following:</span></span>

- [<span data-ttu-id="aeec9-110">Vytváření ukázka Identity</span><span class="sxs-lookup"><span data-stu-id="aeec9-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="aeec9-111">Nastavení služby SMS pro dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="aeec9-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="aeec9-112">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="aeec9-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="aeec9-113">Postup registrace zprostředkovatele dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="aeec9-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="aeec9-114">Kombinování sociálních a místní přihlášení účtů</span><span class="sxs-lookup"><span data-stu-id="aeec9-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="aeec9-115">Uzamčení účtu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="aeec9-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="aeec9-116">Vytváření ukázka Identity</span><span class="sxs-lookup"><span data-stu-id="aeec9-116">Building the Identity sample</span></span>

<span data-ttu-id="aeec9-117">V této části použijete NuGet Stáhnout ukázku, kterou budeme pracovat s.</span><span class="sxs-lookup"><span data-stu-id="aeec9-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="aeec9-118">Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="aeec9-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="aeec9-119">Instalaci sady Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="aeec9-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="aeec9-120">Upozornění: Je nutné nainstalovat Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="aeec9-121">Vytvořte novou ***prázdný*** webový projekt ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="aeec9-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="aeec9-122">V konzole Správce balíčků, zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="aeec9-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="aeec9-123">V tomto kurzu použijeme [sendgrid vám umožňuje](http://sendgrid.com/) k odesílání e-mailu a [Twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) pro odesílání textových zpráv sms.</span><span class="sxs-lookup"><span data-stu-id="aeec9-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="aeec9-124">`Identity.Samples` Balíček nainstaluje kód Pracujeme s.</span><span class="sxs-lookup"><span data-stu-id="aeec9-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="aeec9-125">Nastavte [projektu pro použití protokolu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="aeec9-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="aeec9-126">*Volitelné*: postupujte podle pokynů v mé [kurzu potvrzení e-mailu](account-confirmation-and-password-recovery-with-aspnet-identity.md) spojit sendgrid vám umožňuje a pak spusťte aplikaci a zaregistrovat e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="aeec9-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="aeec9-127">* Volitelné: * kód Ukázkový odkaz e-mailu potvrzení odebrání vzorku ( `ViewBag.Link` kódu v kontroleru účtu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="aeec9-128">Najdete v článku `DisplayEmail` a `ForgotPasswordConfirmation` metody akce a zobrazení syntaxe razor).</span><span class="sxs-lookup"><span data-stu-id="aeec9-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="aeec9-129"><em>Volitelné: * Odebrat `ViewBag.Status` kód z řadičů spravovat a účet a *Views\Account\VerifyCode.cshtml</em> a <em>Views\Manage\VerifyPhoneNumber.cshtml</em> zobrazení syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="aeec9-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="aeec9-130">Alternativně můžete ponechat `ViewBag.Status` displej tak, aby testovat, jak se tato aplikace funguje místně bez nutnosti spojit a odeslat e-mailu a zpráv SMS.</span><span class="sxs-lookup"><span data-stu-id="aeec9-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="aeec9-131">Upozornění: Pokud změníte některá nastavení zabezpečení v této ukázce, výroby, které aplikace bude muset podstoupit auditu zabezpečení, která explicitně volá se změny provedené.</span><span class="sxs-lookup"><span data-stu-id="aeec9-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="aeec9-132">Nastavení služby SMS pro dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="aeec9-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="aeec9-133">Tento kurz obsahuje pokyny pro použití Twilio nebo ASPSMS ale můžete použít další poskytovatele serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="aeec9-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="aeec9-134">**Vytvoření uživatelského účtu s poskytovatelem služby SMS**</span><span class="sxs-lookup"><span data-stu-id="aeec9-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="aeec9-135">Vytvoření [Twilio](https://www.twilio.com/try-twilio) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) účtu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="aeec9-136">**Instalace dalších balíčků nebo přidání odkazů na služby**</span><span class="sxs-lookup"><span data-stu-id="aeec9-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="aeec9-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="aeec9-137">Twilio:</span></span>  
   <span data-ttu-id="aeec9-138">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="aeec9-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="aeec9-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="aeec9-139">ASPSMS:</span></span>  
   <span data-ttu-id="aeec9-140">Následující odkaz na službu musí být přidán:</span><span class="sxs-lookup"><span data-stu-id="aeec9-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="aeec9-141">Adresa:</span><span class="sxs-lookup"><span data-stu-id="aeec9-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="aeec9-142">Obor názvů:</span><span class="sxs-lookup"><span data-stu-id="aeec9-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="aeec9-143">**Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**</span><span class="sxs-lookup"><span data-stu-id="aeec9-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="aeec9-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="aeec9-144">Twilio:</span></span>  
   <span data-ttu-id="aeec9-145">Z **řídicí panel** kartě svého účtu Twilio kopie **SID účtu** a **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="aeec9-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="aeec9-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="aeec9-146">ASPSMS:</span></span>  
   <span data-ttu-id="aeec9-147">V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej spolu s samoobslužné definované **heslo**.</span><span class="sxs-lookup"><span data-stu-id="aeec9-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="aeec9-148">Tyto hodnoty jsme později budou ukládat do proměnné `SMSAccountIdentification` a `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="aeec9-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="aeec9-149">**Zadání ID odesílatele nebo původce**</span><span class="sxs-lookup"><span data-stu-id="aeec9-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="aeec9-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="aeec9-150">Twilio:</span></span>  
   <span data-ttu-id="aeec9-151">Z **čísla** kartě, zkopírujte své telefonní číslo Twilio.</span><span class="sxs-lookup"><span data-stu-id="aeec9-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="aeec9-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="aeec9-152">ASPSMS:</span></span>  
   <span data-ttu-id="aeec9-153">V rámci **odemknutí autoru** nabídce odemknutí jeden nebo více autoru nebo vyberte alfanumerické původce (nepodporuje všechny sítě).</span><span class="sxs-lookup"><span data-stu-id="aeec9-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="aeec9-154">Tato hodnota jsme později se uloží v proměnné `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="aeec9-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="aeec9-155">**Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**</span><span class="sxs-lookup"><span data-stu-id="aeec9-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="aeec9-156">Zkontrolujte přihlašovací údaje a telefonní číslo odesílatele, k dispozici pro aplikaci:</span><span class="sxs-lookup"><span data-stu-id="aeec9-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="aeec9-157">Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="aeec9-158">Účet a přihlašovací údaje se přidají do výše pro zjednodušení v ukázkovém kódu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="aeec9-159">V tématu Jon Atten [rozhraní ASP.NET MVC: zachovat mimo privátní nastavení správy zdrojového kódu](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="aeec9-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="aeec9-160">**Implementace přenos dat do poskytovatele služby SMS**</span><span class="sxs-lookup"><span data-stu-id="aeec9-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="aeec9-161">Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="aeec9-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="aeec9-162">V závislosti na použité poskytovatele služby SMS aktivovat buď **Twilio** nebo **ASPSMS** části:</span><span class="sxs-lookup"><span data-stu-id="aeec9-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="aeec9-163">Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve registrován.</span><span class="sxs-lookup"><span data-stu-id="aeec9-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="aeec9-164">Klikněte na vaše ID uživatele, který aktivuje `Index` metodu akce v `Manage` řadiče.</span><span class="sxs-lookup"><span data-stu-id="aeec9-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="aeec9-165">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="aeec9-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="aeec9-166">Během pár sekund obdržíte textovou zprávu s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="aeec9-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="aeec9-167">Zadejte ho a stiskněte klávesu **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="aeec9-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="aeec9-168">Správa zobrazení ukazuje, že vaše telefonní číslo bylo přidáno.</span><span class="sxs-lookup"><span data-stu-id="aeec9-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="aeec9-169">Zkontrolujte kód</span><span class="sxs-lookup"><span data-stu-id="aeec9-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="aeec9-170">`Index` Metodu akce v `Manage` řadiče nastaví stavové zprávy založené na předchozí akce a poskytuje odkazy na místní heslo změnit nebo přidat místní účet.</span><span class="sxs-lookup"><span data-stu-id="aeec9-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="aeec9-171">`Index` Metoda také zobrazuje stav nebo vaše 2FA telefonní číslo, externích přihlášení, 2FA povolena a nezapomeňte 2FA metodu pro tento prohlížeč (vysvětlení později).</span><span class="sxs-lookup"><span data-stu-id="aeec9-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="aeec9-172">Kliknutím na vaše ID uživatele (e-mailu) v záhlaví neprojde zprávu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="aeec9-173">Kliknutím na **telefonní číslo: odeberte** odkaz předává `Message=RemovePhoneSuccess` jako řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="aeec9-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="aeec9-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="aeec9-175">`AddPhoneNumber` Metoda akce zobrazí dialogové okno zadat telefonní číslo, které může přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="aeec9-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="aeec9-176">Kliknutím na **Odeslat ověřovací kód** tlačítko účtuje telefonní číslo na HTTP POST `AddPhoneNumber` metody akce.</span><span class="sxs-lookup"><span data-stu-id="aeec9-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="aeec9-177">`GenerateChangePhoneNumberTokenAsync` Metoda generuje tokeny zabezpečení, které budou nastaveny ve zprávě SMS.</span><span class="sxs-lookup"><span data-stu-id="aeec9-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="aeec9-178">Pokud je nakonfigurovaná služba SMS, token se budou odesílat jako řetězec &quot;váš kód zabezpečení: &lt;tokenu&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="aeec9-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="aeec9-179">`SmsService.SendAsync` Metodu nazývá asynchronně a potom aplikaci je přesměrován na `VerifyPhoneNumber` metoda akce (který se zobrazí následující dialogové okno), kde můžete zadat ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="aeec9-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="aeec9-180">Poté zadejte kód a klikněte na odeslání, kód je odeslána do HTTP POST `VerifyPhoneNumber` metody akce.</span><span class="sxs-lookup"><span data-stu-id="aeec9-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="aeec9-181">`ChangePhoneNumberAsync` Metoda zkontroluje odeslaných bezpečnostní kód.</span><span class="sxs-lookup"><span data-stu-id="aeec9-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="aeec9-182">Pokud kód je správná, telefonní číslo je přidat do `PhoneNumber` pole z `AspNetUsers` tabulky.</span><span class="sxs-lookup"><span data-stu-id="aeec9-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="aeec9-183">Pokud toto volání úspěšné, `SignInAsync` metoda je volána:</span><span class="sxs-lookup"><span data-stu-id="aeec9-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="aeec9-184">`isPersistent` Parametr nastaví, zda je relace ověřování zachová pro víc požadavků.</span><span class="sxs-lookup"><span data-stu-id="aeec9-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="aeec9-185">Když změníte profil zabezpečení, nové razítko zabezpečení se generuje a uloží do `SecurityStamp` pole z *AspNetUsers* tabulky.</span><span class="sxs-lookup"><span data-stu-id="aeec9-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="aeec9-186">Poznámka: `SecurityStamp` pole se liší od zabezpečení souboru cookie.</span><span class="sxs-lookup"><span data-stu-id="aeec9-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="aeec9-187">Soubor cookie zabezpečení nejsou uloženy v `AspNetUsers` tabulky (nebo kdekoli jinde v databázi Identity).</span><span class="sxs-lookup"><span data-stu-id="aeec9-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="aeec9-188">Token zabezpečení souboru cookie je podepsaný pomocí [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) a pomocí `UserId, SecurityStamp` a informace o čas vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="aeec9-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="aeec9-189">Middlewaru souboru cookie zkontroluje souborů cookie u každého požadavku.</span><span class="sxs-lookup"><span data-stu-id="aeec9-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="aeec9-190">`SecurityStampValidator` Metoda v `Startup` třída přístupů do databáze a zkontroluje razítko zabezpečení pravidelně jako zadaný `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="aeec9-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="aeec9-191">Tato situace nastane pouze každých 30 minut (v našem ukázce) není-li změnit váš profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="aeec9-192">Chcete-li minimalizovat služebních cest do databáze byla zvolena intervalu 30 minut.</span><span class="sxs-lookup"><span data-stu-id="aeec9-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="aeec9-193">`SignInAsync` Metoda musí být volána při jakékoli změně profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="aeec9-194">Při změně profil zabezpečení databáze je aktualizace `SecurityStamp` pole a bez volání `SignInAsync` metoda by zůstat přihlášení *pouze* do příštího kanálu OWIN přístupů do databáze ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="aeec9-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="aeec9-195">Toto můžete otestovat změnou `SignInAsync` metoda vrátí okamžitě a nastavení souboru cookie `validateInterval` vlastnost z 30 minut na 5 sekund:</span><span class="sxs-lookup"><span data-stu-id="aeec9-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="aeec9-196">Výše uvedený kód změn, můžete změnit váš profil zabezpečení (například změnou stavu **dvě povolené Multi-Factor**) a můžete se odhlásit, v 5 sekund při `SecurityStampValidator.OnValidateIdentity` metoda selže.</span><span class="sxs-lookup"><span data-stu-id="aeec9-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="aeec9-197">Odebrat návratový řádek v `SignInAsync` metoda, nastavit jiný profil zabezpečení změnit a jste se nebudou protokolovat. `SignInAsync` Metoda generuje nový soubor cookie zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="aeec9-198">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="aeec9-198">Enable two-factor authentication</span></span>

<span data-ttu-id="aeec9-199">V ukázkové aplikace budete muset povolit dvoufaktorové ověřování (2FA) pomocí uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="aeec9-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="aeec9-200">Chcete-li povolit 2FA, klikněte na vaše ID uživatele (e-mailový alias) na navigačním panelu.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="aeec9-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="aeec9-201">Klikněte na povolit 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="aeec9-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="aeec9-202">Odhlaste se a potom znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="aeec9-202">Log out, then log back in.</span></span> <span data-ttu-id="aeec9-203">Pokud jste povolili e-mailu (najdete v části Moje [předchozí kurzu](account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat SMS nebo e-mailu pro 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="aeec9-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="aeec9-204">Zobrazí se stránka ověření kódu, kde můžete zadat kód (z SMS nebo e-mailu).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="aeec9-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="aeec9-205">Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení pomocí tohoto počítače a prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="aeec9-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="aeec9-206">Povolení 2FA a kliknutím na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k vašemu počítači.</span><span class="sxs-lookup"><span data-stu-id="aeec9-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="aeec9-207">Můžete provést z jakéhokoli privátní počítače, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="aeec9-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="aeec9-208">Nastavením **mějte na paměti, tento prohlížeč**, můžete získat z počítače, které nepoužívají pravidelně zvýšené zabezpečení 2FA a získat užitečný na nebudou muset projít 2FA na vlastní počítače.</span><span class="sxs-lookup"><span data-stu-id="aeec9-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="aeec9-209">Postup registrace zprostředkovatele dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="aeec9-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="aeec9-210">Když vytvoříte nový projekt MVC, *IdentityConfig.cs* soubor obsahuje následující kód k registraci zprostředkovatele dvoufaktorového ověřování:</span><span class="sxs-lookup"><span data-stu-id="aeec9-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="aeec9-211">Přidat telefonní číslo pro 2FA</span><span class="sxs-lookup"><span data-stu-id="aeec9-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="aeec9-212">`AddPhoneNumber` Metodu akce v `Manage` zadané jeho telefonní číslo, které se odesílají a řadič generuje token zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="aeec9-213">Po odeslání token, je přesměrován na `VerifyPhoneNumber` metody akce, kde můžete zadat kód k registraci serveru SMS pro 2FA.</span><span class="sxs-lookup"><span data-stu-id="aeec9-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="aeec9-214">SMS 2FA se nepoužije, dokud si neověříte telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="aeec9-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="aeec9-215">Povolení 2FA</span><span class="sxs-lookup"><span data-stu-id="aeec9-215">Enabling 2FA</span></span>

<span data-ttu-id="aeec9-216">`EnableTFA` Metoda akce umožňuje 2FA:</span><span class="sxs-lookup"><span data-stu-id="aeec9-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="aeec9-217">Poznámka: `SignInAsync` musí volat, protože je povolit 2FA ke změně profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="aeec9-218">Pokud je povoleno 2FA, uživatel bude muset použít 2FA k přihlášení přístupy 2FA registrace (SMS a e-mailu v ukázce).</span><span class="sxs-lookup"><span data-stu-id="aeec9-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="aeec9-219">Se dají přidávat další zprostředkovatelé 2FA například generátory kódu QR nebo můžete napsat vlastníte (viz [pomocí Google Authenticator s ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="aeec9-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="aeec9-220">Kódy 2FA jsou generovány pomocí [založené na čase jednorázové heslo algoritmus](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) a kódy jsou platné po dobu šesti minut.</span><span class="sxs-lookup"><span data-stu-id="aeec9-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="aeec9-221">Pokud pořídíte víc než šest minut zadejte kód, získáte neplatný kód chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="aeec9-222">Kombinování sociálních a místní přihlášení účtů</span><span class="sxs-lookup"><span data-stu-id="aeec9-222">Combine social and local login accounts</span></span>

<span data-ttu-id="aeec9-223">Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty.</span><span class="sxs-lookup"><span data-stu-id="aeec9-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="aeec9-224">V tomto pořadí &quot; RickAndMSFT@gmail.com &quot; je poprvé vytvořen jako místní přihlášení, ale můžete vytvořit účet jako sociálních protokolu v prvním a pak přidejte místní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="aeec9-225">Klikněte na **spravovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="aeec9-225">Click on the **Manage** link.</span></span> <span data-ttu-id="aeec9-226">Poznámka: externí 0 (sociální přihlášení) spojené s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="aeec9-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="aeec9-227">Klikněte na odkaz na jiný protokol ve službě service a přijímat žádosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="aeec9-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="aeec9-228">Dva účty jsou spojena, nebudete moci přihlásit pomocí buď účtu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="aeec9-229">Můžete chtít uživatelům v případě, že jejich sociální přihlášení ověřovací služby, jsou vypnuty nebo jejich více pravděpodobně ztratili přístup k účtu mají sociálních přidat místní účty.</span><span class="sxs-lookup"><span data-stu-id="aeec9-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="aeec9-230">Na následujícím obrázku je tní sociálních přihlášení (což je vidět na **externích přihlášení: 1** zobrazený na stránce).</span><span class="sxs-lookup"><span data-stu-id="aeec9-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="aeec9-231">Kliknutím na **vyberte heslo** umožňuje přidat místního protokolu na související se stejným účtem.</span><span class="sxs-lookup"><span data-stu-id="aeec9-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="aeec9-232">Uzamčení účtu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="aeec9-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="aeec9-233">Účty ve vaší aplikaci z slovníkovým útokům můžete chránit povolením uzamčení uživatele.</span><span class="sxs-lookup"><span data-stu-id="aeec9-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="aeec9-234">Následující kód na `ApplicationUserManager Create` metoda nakonfiguruje uzamčení:</span><span class="sxs-lookup"><span data-stu-id="aeec9-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="aeec9-235">Výše uvedený kód umožňuje uzamčení pro jenom dvoufaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="aeec9-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="aeec9-236">Když povolíte uzamčení pro přihlášení změnou `shouldLockout` na hodnotu true v `Login` metoda řadičem účet, doporučujeme uzamčení pro přihlášení není povolit, protože účet je ohrožena útoky založenými na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) přihlášení útoky.</span><span class="sxs-lookup"><span data-stu-id="aeec9-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="aeec9-237">V ukázkovém kódu je zakázána uzamčení pro účet správce vytvořený v `ApplicationDbInitializer Seed` metoda:</span><span class="sxs-lookup"><span data-stu-id="aeec9-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="aeec9-238">A uživatel tak, aby měl ověřené e-mailový účet</span><span class="sxs-lookup"><span data-stu-id="aeec9-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="aeec9-239">Následující kód, musí uživatel tak, aby měl ověřené e-mailový účet, než se můžete přihlásit:</span><span class="sxs-lookup"><span data-stu-id="aeec9-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="aeec9-240">Jak SignInManager vyhledává 2FA požadavek</span><span class="sxs-lookup"><span data-stu-id="aeec9-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="aeec9-241">Jak místní přihlášení a sociálních protokolu v zkontrolujte, zda je povoleno 2FA.</span><span class="sxs-lookup"><span data-stu-id="aeec9-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="aeec9-242">Pokud je povoleno 2FA, `SignInManager` přihlášení metoda vrátí `SignInStatus.RequiresVerification`, a uživatel bude přesměrován na `SendCode` metody akce, které se mají zadat kód pro dokončení protokol v pořadí.</span><span class="sxs-lookup"><span data-stu-id="aeec9-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="aeec9-243">Pokud má uživatel obdobou je nastavený na místní soubor cookie uživatele, `SignInManager` vrátí `SignInStatus.Success` a nebude muset projít 2FA.</span><span class="sxs-lookup"><span data-stu-id="aeec9-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="aeec9-244">Následující kód ukazuje `SendCode` metody akce.</span><span class="sxs-lookup"><span data-stu-id="aeec9-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="aeec9-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) je vytvořena s všechny metody 2FA pro daného uživatele povoleno.</span><span class="sxs-lookup"><span data-stu-id="aeec9-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="aeec9-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) je předán [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocné rutiny, která umožňuje uživateli vybrat metodu 2FA (obvykle e-mailu a SMS).</span><span class="sxs-lookup"><span data-stu-id="aeec9-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="aeec9-247">Jakmile uživatel odešle 2FA přístup, `HTTP POST SendCode` akce metoda je volána, `SignInManager` zasílá kód 2FA a uživatel se přesměruje na `VerifyCode` metody akce, kde můžete zadat kód pro dokončení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="aeec9-248">2FA uzamčení</span><span class="sxs-lookup"><span data-stu-id="aeec9-248">2FA Lockout</span></span>

<span data-ttu-id="aeec9-249">I když selhání pokusů o přihlášení hesel můžete nastavit uzamčení účtu, tento přístup umožňuje vaše přihlášení náchylné k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) uzamčení.</span><span class="sxs-lookup"><span data-stu-id="aeec9-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="aeec9-250">Doporučujeme, abyste že je použít pouze s 2FA uzamčení účtu.</span><span class="sxs-lookup"><span data-stu-id="aeec9-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="aeec9-251">Když `ApplicationUserManager` je vytvořen, ukázkový kód nastaví 2FA uzamčení a `MaxFailedAccessAttemptsBeforeLockout` na pět.</span><span class="sxs-lookup"><span data-stu-id="aeec9-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="aeec9-252">Jakmile se uživatel přihlásí (prostřednictvím místního účtu nebo sociální účtu), je uložen v 2FA každý neúspěšný pokus a pokud bude dosažen maximální počet pokusů, uživatel je uzamčen pět minut (můžete nastavit uzamčení časem `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="aeec9-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="aeec9-253">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="aeec9-253">Additional Resources</span></span>

- <span data-ttu-id="aeec9-254">[ASP.NET Identity doporučené prostředky](../getting-started/aspnet-identity-recommended-resources.md) úplný seznam Identity blogy, videa, kurzy a dobře tak odkazy.</span><span class="sxs-lookup"><span data-stu-id="aeec9-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="aeec9-255">[Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) také ukazuje, jak přidat informace o profilu do tabulky uživatelů.</span><span class="sxs-lookup"><span data-stu-id="aeec9-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="aeec9-256">[ASP.NET MVC a Identity 2.0: Principy Základy](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) podle Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="aeec9-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="aeec9-257">Potvrzení účtu a heslo pro obnovení s ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="aeec9-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="aeec9-258">Úvod do ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="aeec9-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="aeec9-259">[Uvedení RTM technologie ASP.NET identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) podle Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="aeec9-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="aeec9-260">[Identitu ASP.NET 2.0: Nastavení ověření účtu a dvoufaktorové ověřování](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) podle Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="aeec9-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
