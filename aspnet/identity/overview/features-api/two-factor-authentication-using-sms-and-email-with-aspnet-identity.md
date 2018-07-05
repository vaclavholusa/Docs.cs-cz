---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity | Dokumentace Microsoftu
author: HaoK
description: Tento kurzu se dozvíte, jak nastavit dvojúrovňového ověřování (2FA) pomocí SMS a e-mailu. Tento článek zapsal Rick Anderson ( @RickAndMSFT ), za...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0519ae69b3d2ef129d206a936b199f781fef5bf6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399676"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="0a00f-104">Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="0a00f-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="0a00f-105">podle [Haovi společnosti ani](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="0a00f-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="0a00f-106">Tento kurzu se dozvíte, jak nastavit dvojúrovňového ověřování (2FA) pomocí SMS a e-mailu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="0a00f-107">Tento článek zapsal Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), ani Haovi společnosti a Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="0a00f-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="0a00f-108">Ukázka NuGet zapsal primárně ani Haovi společnosti.</span><span class="sxs-lookup"><span data-stu-id="0a00f-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="0a00f-109">Toto téma obsahuje následující:</span><span class="sxs-lookup"><span data-stu-id="0a00f-109">This topic covers the following:</span></span>

- [<span data-ttu-id="0a00f-110">Ukázka Identity sestavení</span><span class="sxs-lookup"><span data-stu-id="0a00f-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="0a00f-111">Nastavení služby SMS pro dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0a00f-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="0a00f-112">Povolení dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0a00f-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="0a00f-113">Postup při registraci zprostředkovatele dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0a00f-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="0a00f-114">Sloučit účty sociálních sítí a místní přihlášení</span><span class="sxs-lookup"><span data-stu-id="0a00f-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="0a00f-115">Uzamčení účtu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="0a00f-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="0a00f-116">Ukázka Identity sestavení</span><span class="sxs-lookup"><span data-stu-id="0a00f-116">Building the Identity sample</span></span>

<span data-ttu-id="0a00f-117">V této části použijete Stáhnout ukázku, kterou pak ve spolupráci s NuGet.</span><span class="sxs-lookup"><span data-stu-id="0a00f-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="0a00f-118">Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="0a00f-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="0a00f-119">Instalace sady Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="0a00f-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="0a00f-120">Upozornění: Je nutné nainstalovat Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="0a00f-121">Vytvořte nový ***prázdný*** webový projekt ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0a00f-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="0a00f-122">V konzole Správce balíčků zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="0a00f-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="0a00f-123">V tomto kurzu použijeme [SendGrid](http://sendgrid.com/) k odesílání e-mailu a [Twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) pro odesílání textových zpráv sms.</span><span class="sxs-lookup"><span data-stu-id="0a00f-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="0a00f-124">`Identity.Samples` Balíček nainstaluje budeme pracovat s kódem.</span><span class="sxs-lookup"><span data-stu-id="0a00f-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="0a00f-125">Nastavte [projektu pro použití protokolu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="0a00f-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="0a00f-126">*Volitelné*: postupujte podle pokynů v mé [kurzu potvrzení e-mailu](account-confirmation-and-password-recovery-with-aspnet-identity.md) připojení SendGrid a pak spusťte aplikaci a zaregistrování e-mailový účet.</span><span class="sxs-lookup"><span data-stu-id="0a00f-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="0a00f-127">* Volitelné: * odebrat je ukázka e-mailu odkaz potvrzovací kód z ukázkového ( `ViewBag.Link` kód v účet kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a00f-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="0a00f-128">Zobrazit `DisplayEmail` a `ForgotPasswordConfirmation` metody akce a zobrazeními razor).</span><span class="sxs-lookup"><span data-stu-id="0a00f-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="0a00f-129"><em>Volitelné: * Odebrat `ViewBag.Status` kódu z řadiče spravovat a účet a *Views\Account\VerifyCode.cshtml</em> a <em>Views\Manage\VerifyPhoneNumber.cshtml</em> zobrazení syntaxe razor.</span><span class="sxs-lookup"><span data-stu-id="0a00f-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="0a00f-130">Alternativně můžete zachovat `ViewBag.Status` displej tak, aby test, jak tato aplikace funguje místně bez nutnosti připojení a odeslat e-mailu a zpráv SMS.</span><span class="sxs-lookup"><span data-stu-id="0a00f-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="0a00f-131">Upozornění: Pokud změníte některá nastavení zabezpečení v této ukázce, výroby aplikací bude chtít projít auditu zabezpečení, který explicitně vám sdělí, provedené změny.</span><span class="sxs-lookup"><span data-stu-id="0a00f-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="0a00f-132">Nastavení služby SMS pro dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0a00f-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="0a00f-133">Tento kurz obsahuje pokyny k používání Twilio nebo ASPSMS ale můžete použít jakýkoli jiný poskytovatel serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="0a00f-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="0a00f-134">**Vytvoření uživatelského účtu s poskytovatelem služby SMS**</span><span class="sxs-lookup"><span data-stu-id="0a00f-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="0a00f-135">Vytvoření [Twilio](https://www.twilio.com/try-twilio) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) účtu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="0a00f-136">**Instalace dalších balíčků nebo přidávání odkazů na služby**</span><span class="sxs-lookup"><span data-stu-id="0a00f-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="0a00f-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="0a00f-137">Twilio:</span></span>  
   <span data-ttu-id="0a00f-138">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0a00f-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="0a00f-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="0a00f-139">ASPSMS:</span></span>  
   <span data-ttu-id="0a00f-140">Následující odkaz na službu musí být přidán:</span><span class="sxs-lookup"><span data-stu-id="0a00f-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="0a00f-141">Adresa:</span><span class="sxs-lookup"><span data-stu-id="0a00f-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="0a00f-142">Obor názvů:</span><span class="sxs-lookup"><span data-stu-id="0a00f-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="0a00f-143">**Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**</span><span class="sxs-lookup"><span data-stu-id="0a00f-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="0a00f-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="0a00f-144">Twilio:</span></span>  
   <span data-ttu-id="0a00f-145">Z **řídicí panel** kartu svého účtu Twilio, kopie **SID účtu** a **ověřovací token**.</span><span class="sxs-lookup"><span data-stu-id="0a00f-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="0a00f-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="0a00f-146">ASPSMS:</span></span>  
   <span data-ttu-id="0a00f-147">V nastavení účtu, přejděte na **vlastnosti userkey jedná** a zkopírujte ho spolu s místním definované **heslo**.</span><span class="sxs-lookup"><span data-stu-id="0a00f-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="0a00f-148">Uložíme tyto hodnoty se později v proměnné `SMSAccountIdentification` a `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="0a00f-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="0a00f-149">**Určení SenderID / původce**</span><span class="sxs-lookup"><span data-stu-id="0a00f-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="0a00f-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="0a00f-150">Twilio:</span></span>  
   <span data-ttu-id="0a00f-151">Z **čísla** kartu, zkopírujte své telefonní číslo Twilio.</span><span class="sxs-lookup"><span data-stu-id="0a00f-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="0a00f-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="0a00f-152">ASPSMS:</span></span>  
   <span data-ttu-id="0a00f-153">V rámci **odemknout autoru** nabídce odemknout jeden nebo více autoru nebo zvolte příkazce alfanumerické znaky (není podporováno ve všech sítích).</span><span class="sxs-lookup"><span data-stu-id="0a00f-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="0a00f-154">Jsme později uloží tuto hodnotu do proměnné `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="0a00f-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="0a00f-155">**Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**</span><span class="sxs-lookup"><span data-stu-id="0a00f-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="0a00f-156">Přihlašovací údaje a zpřístupníte odesílatele telefonní číslo do aplikace:</span><span class="sxs-lookup"><span data-stu-id="0a00f-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="0a00f-157">Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="0a00f-158">Účet a přihlašovací údaje se přidají do výše uvedený pro zjednodušení ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="0a00f-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="0a00f-159">Zobrazit Jon Atten [ASP.NET MVC: zachovat privátní nastavení vzdálené správy zdrojového kódu](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="0a00f-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="0a00f-160">**Provádění přenos dat do poskytovatele služby SMS**</span><span class="sxs-lookup"><span data-stu-id="0a00f-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="0a00f-161">Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="0a00f-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="0a00f-162">V závislosti na poskytovateli serveru SMS používá aktivaci buď **Twilio** nebo **ASPSMS** části:</span><span class="sxs-lookup"><span data-stu-id="0a00f-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="0a00f-163">Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve zaregistrovali.</span><span class="sxs-lookup"><span data-stu-id="0a00f-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="0a00f-164">Klikněte na své ID uživatele, která se aktivuje `Index` metodu akce v `Manage` kontroleru.</span><span class="sxs-lookup"><span data-stu-id="0a00f-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="0a00f-165">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="0a00f-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="0a00f-166">Během několika sekund obdržíte textovou zprávu s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="0a00f-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="0a00f-167">Zadejte ji a stiskněte klávesu **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="0a00f-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="0a00f-168">Správa zobrazení ukazuje že vaše telefonní číslo bylo přidáno.</span><span class="sxs-lookup"><span data-stu-id="0a00f-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="0a00f-169">Prozkoumejte kód</span><span class="sxs-lookup"><span data-stu-id="0a00f-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="0a00f-170">`Index` Metodu akce v `Manage` kontroleru nastaví ve zprávě o stavu na základě předchozí akce a poskytuje odkazy na vaše místní heslo změnit nebo přidat místní účet.</span><span class="sxs-lookup"><span data-stu-id="0a00f-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="0a00f-171">`Index` Metoda také zobrazí stav nebo váš 2FA telefonní číslo, externí přihlášení, 2FA povolená a nezapomeňte 2FA metoda tohoto prohlížeče (vysvětleno dále).</span><span class="sxs-lookup"><span data-stu-id="0a00f-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="0a00f-172">Kliknutím na své ID uživatele (e-mail) v záhlaví není úspěšný zprávu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="0a00f-173">Kliknutím na **telefonní číslo: odeberte** propojit předá `Message=RemovePhoneSuccess` jako řetězec dotazu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="0a00f-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="0a00f-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="0a00f-175">`AddPhoneNumber` Metoda akce se zobrazí dialogové okno zadat telefonní číslo, které můžou přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="0a00f-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="0a00f-176">Kliknutím na **poslat ověřovací kód** tlačítko odešle telefonní číslo do HTTP POST `AddPhoneNumber` metody akce.</span><span class="sxs-lookup"><span data-stu-id="0a00f-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="0a00f-177">`GenerateChangePhoneNumberTokenAsync` Metoda generuje token zabezpečení, který bude nastavený ve zprávě SMS.</span><span class="sxs-lookup"><span data-stu-id="0a00f-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="0a00f-178">Pokud služba SMS byla nakonfigurovaná, token, který se odešle jako řetězec &quot;je váš bezpečnostní kód &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="0a00f-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="0a00f-179">`SmsService.SendAsync` Metoda je volána asynchronně a potom aplikaci je přesměrován na `VerifyPhoneNumber` metody akce (který se zobrazí následující dialogové okno), ve kterém můžete zadat ověřovací kód.</span><span class="sxs-lookup"><span data-stu-id="0a00f-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="0a00f-180">Zadejte kód a klikněte na Odeslat kód je odeslána do HTTP POST `VerifyPhoneNumber` metody akce.</span><span class="sxs-lookup"><span data-stu-id="0a00f-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="0a00f-181">`ChangePhoneNumberAsync` Metoda zkontroluje odeslaných bezpečnostní kód.</span><span class="sxs-lookup"><span data-stu-id="0a00f-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="0a00f-182">Pokud kód je správný, telefonní číslo je přidána do `PhoneNumber` pole `AspNetUsers` tabulky.</span><span class="sxs-lookup"><span data-stu-id="0a00f-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="0a00f-183">Pokud toto volání je úspěšné, `SignInAsync` volání metody:</span><span class="sxs-lookup"><span data-stu-id="0a00f-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="0a00f-184">`isPersistent` Parametr nastaví, zda je relace ověřování zachová pro víc požadavků.</span><span class="sxs-lookup"><span data-stu-id="0a00f-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="0a00f-185">Pokud změníte profil zabezpečení, je generována a uložené v nové razítko zabezpečení `SecurityStamp` pole *AspNetUsers* tabulky.</span><span class="sxs-lookup"><span data-stu-id="0a00f-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="0a00f-186">Všimněte si, `SecurityStamp` pole se liší od soubor cookie zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="0a00f-187">Soubor cookie zabezpečení není uložený v `AspNetUsers` tabulky (nebo kdekoli jinde v Identity DB).</span><span class="sxs-lookup"><span data-stu-id="0a00f-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="0a00f-188">Token souboru cookie zabezpečení je podepsaný držitelem, pomocí [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) a je vytvořen s `UserId, SecurityStamp` a informace doby vypršení platnosti.</span><span class="sxs-lookup"><span data-stu-id="0a00f-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="0a00f-189">Middlewaru souboru cookie. zkontroluje soubor cookie s každým požadavkem.</span><span class="sxs-lookup"><span data-stu-id="0a00f-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="0a00f-190">`SecurityStampValidator` Metodu `Startup` třídy volání databáze a zkontroluje razítko zabezpečení pravidelně uvedené s `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="0a00f-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="0a00f-191">Tato situace nastane pouze každých 30 minut (v našem příkladu) nezměníte svůj profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="0a00f-192">Chcete-li minimalizovat cest k databázi bylo zvoleno intervalu 30 minut.</span><span class="sxs-lookup"><span data-stu-id="0a00f-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="0a00f-193">`SignInAsync` Metoda musí být volána při jakékoli změně na profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="0a00f-194">Když se změní na profil zabezpečení databáze je aktualizace `SecurityStamp` pole a bez volání `SignInAsync` metody by zůstat přihlášen(a) *pouze* až při příští kanálu OWIN narazí na databázi ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="0a00f-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="0a00f-195">Můžete ho otestovat tak, že změníte `SignInAsync` metoda výsledky okamžitě a nastavení souboru cookie `validateInterval` vlastnost z 30 minut na 5 sekund:</span><span class="sxs-lookup"><span data-stu-id="0a00f-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="0a00f-196">Výše uvedené změny kódu, můžete změnit svůj profil zabezpečení (například tak, že změníte stav **dvě povolené faktor**) a můžete se odhlásíte během 5 sekund při `SecurityStampValidator.OnValidateIdentity` metoda selže.</span><span class="sxs-lookup"><span data-stu-id="0a00f-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="0a00f-197">Odebrání řádku vrácené v `SignInAsync` metoda, nastavit jiný profil zabezpečení změnit a můžete se nebudou protokolovat navýšení kapacity. `SignInAsync` Metoda vytvoří nový soubor cookie zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="0a00f-198">Povolení dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0a00f-198">Enable two-factor authentication</span></span>

<span data-ttu-id="0a00f-199">V ukázkové aplikaci musíte použít uživatelské rozhraní pro povolení dvoufaktorového ověřování (2FA).</span><span class="sxs-lookup"><span data-stu-id="0a00f-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="0a00f-200">Pokud chcete povolit 2FA, klikněte na své ID uživatele (e-mailový alias) na navigačním panelu.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="0a00f-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="0a00f-201">Klikněte na povolit 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="0a00f-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="0a00f-202">Odhlaste se a potom znovu přihlásit.</span><span class="sxs-lookup"><span data-stu-id="0a00f-202">Log out, then log back in.</span></span> <span data-ttu-id="0a00f-203">Pokud jste povolili e-mailu (naleznete v tématu Moje [předchozí kurz o službě](account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat, SMS nebo e-mailu pro 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="0a00f-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="0a00f-204">Zobrazí se stránka Ověřte kód, ve kterém můžete zadat kód (z SMS nebo e-mailu).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="0a00f-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="0a00f-205">Kliknutím na **chcete Zapamatovat tento prohlížeč** zaškrtávacího políčka můžete vyloučit před nutností pomocí 2FA Přihlaste se pomocí tohoto počítače a prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0a00f-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="0a00f-206">Povolení 2FA a kliknete na **chcete Zapamatovat tento prohlížeč** vám poskytne silné 2FA ochrany z uživateli se zlými úmysly pokusu o přístup ke svému účtu, za předpokladu, že nemají přístup k vašemu počítači.</span><span class="sxs-lookup"><span data-stu-id="0a00f-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="0a00f-207">Můžete to provést na privátní počítač, který pravidelně používáte.</span><span class="sxs-lookup"><span data-stu-id="0a00f-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="0a00f-208">Nastavením **chcete Zapamatovat tento prohlížeč**, získáte z počítačů, které nepoužíváte pravidelně zvýšení zabezpečení 2FA, a získáte usnadnění práce na nebudete muset absolvovat 2FA na vlastním počítači.</span><span class="sxs-lookup"><span data-stu-id="0a00f-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="0a00f-209">Postup při registraci zprostředkovatele dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="0a00f-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="0a00f-210">Když vytvoříte nový projekt MVC *IdentityConfig.cs* soubor obsahuje následující kód k registraci zprostředkovatele dvoufaktorového ověřování:</span><span class="sxs-lookup"><span data-stu-id="0a00f-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="0a00f-211">Přidat telefonní číslo pro 2FA</span><span class="sxs-lookup"><span data-stu-id="0a00f-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="0a00f-212">`AddPhoneNumber` Metodu akce v `Manage` řadič generuje token zabezpečení a odešle na telefonní číslo, které se mají k dispozici.</span><span class="sxs-lookup"><span data-stu-id="0a00f-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="0a00f-213">Po odeslání tokenu, přesměruje na `VerifyPhoneNumber` metodě akce, ve kterém můžete zadat kód a zaregistrujte 2FA SMS.</span><span class="sxs-lookup"><span data-stu-id="0a00f-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="0a00f-214">SMS 2FA se nepoužije, dokud si neověříte telefonní číslo.</span><span class="sxs-lookup"><span data-stu-id="0a00f-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="0a00f-215">Povolení 2FA</span><span class="sxs-lookup"><span data-stu-id="0a00f-215">Enabling 2FA</span></span>

<span data-ttu-id="0a00f-216">`EnableTFA` Metody akce umožňuje 2FA:</span><span class="sxs-lookup"><span data-stu-id="0a00f-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="0a00f-217">Poznámka: `SignInAsync` musí volat, protože povolit 2FA se mění na profil zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="0a00f-218">Při zapnutém 2FA, uživatel bude potřebovat k přihlášení pomocí 2FA pomocí 2FA přístupů registrace (SMS a e-mailu v ukázce).</span><span class="sxs-lookup"><span data-stu-id="0a00f-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="0a00f-219">Můžete přidat další poskytovatelé 2FA, jako je například generátory kódu QR nebo můžete napsat, které vlastníte (viz [pomocí Google Authenticator s ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="0a00f-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="0a00f-220">2FA kódy jsou generovány pomocí [podle času jednorázové heslo algoritmus](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) a kódy jsou platné po dobu 6 minut.</span><span class="sxs-lookup"><span data-stu-id="0a00f-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="0a00f-221">Pokud budete postupovat víc než šest minut zadejte kód, získáte neplatný kód chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="0a00f-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="0a00f-222">Sloučit účty sociálních sítí a místní přihlášení</span><span class="sxs-lookup"><span data-stu-id="0a00f-222">Combine social and local login accounts</span></span>

<span data-ttu-id="0a00f-223">Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociální účty.</span><span class="sxs-lookup"><span data-stu-id="0a00f-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="0a00f-224">V následujícím pořadí &quot; RickAndMSFT@gmail.com &quot; se nejprve vytvoří jako místní přihlášení, ale můžete vytvořit účet jako sociální protokolu v prvním a pak přidat místní přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="0a00f-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="0a00f-225">Klikněte na **spravovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="0a00f-225">Click on the **Manage** link.</span></span> <span data-ttu-id="0a00f-226">Poznámka: externí 0 (přihlašování přes sociální sítě) spojená s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="0a00f-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="0a00f-227">Klikněte na odkaz na jiný protokol služby a přijímání požadavků aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a00f-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="0a00f-228">Byli sloučeni dva účty, budou moct přihlásit pomocí obou.</span><span class="sxs-lookup"><span data-stu-id="0a00f-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="0a00f-229">Můžete chtít uživatelům přidat místní účty v případě jejich sociálních sítí protokolu ve službě ověřování je mimo provoz nebo spíše se ztratili přístup k jejich účtu na sociální síti.</span><span class="sxs-lookup"><span data-stu-id="0a00f-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="0a00f-230">Na následujícím obrázku je Petr sociální přihlášení (které si můžete všimnout **externí přihlášení: 1** zobrazený na stránce).</span><span class="sxs-lookup"><span data-stu-id="0a00f-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="0a00f-231">Kliknutím na **výběru hesla** umožňuje přidat místního protokolu na spojené se stejným účtem.</span><span class="sxs-lookup"><span data-stu-id="0a00f-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="0a00f-232">Uzamčení účtu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="0a00f-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="0a00f-233">Dokáže chránit účty v aplikaci na slovníkové útoky tím, že uzamčení uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0a00f-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="0a00f-234">Následující kód na `ApplicationUserManager Create` metoda nakonfiguruje uzamčení:</span><span class="sxs-lookup"><span data-stu-id="0a00f-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="0a00f-235">Výše uvedený kód povoluje uzamčení k dvojúrovňovému ověřování pouze.</span><span class="sxs-lookup"><span data-stu-id="0a00f-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="0a00f-236">Zatímco uzamčení pro přihlášení můžete povolit tak, že změníte `shouldLockout` na hodnotu true `Login` metody kontroleru účet, doporučujeme uzamčení pro přihlášení není povolit, protože účet je náchylný k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) přihlášení útoky.</span><span class="sxs-lookup"><span data-stu-id="0a00f-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="0a00f-237">Ve vzorovém kódu uzamčení zakázána pro účet správce vytvořený `ApplicationDbInitializer Seed` metody:</span><span class="sxs-lookup"><span data-stu-id="0a00f-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="0a00f-238">By po uživateli, aby ověřený e-mailový účet</span><span class="sxs-lookup"><span data-stu-id="0a00f-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="0a00f-239">Následující kód, musí uživatel má ověřená e-mailový účet před můžou přihlásit:</span><span class="sxs-lookup"><span data-stu-id="0a00f-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="0a00f-240">Jak SignInManager vyhledává 2FA požadavek</span><span class="sxs-lookup"><span data-stu-id="0a00f-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="0a00f-241">Místní přihlášení i sociální přihlášení zkontrolujte, jestli je povolené 2FA.</span><span class="sxs-lookup"><span data-stu-id="0a00f-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="0a00f-242">Pokud je povolené 2FA, `SignInManager` přihlašovací metoda vrátí `SignInStatus.RequiresVerification`, a uživatel bude přesměrován na `SendCode` metodě akce, ve kterém bude nutné zadat kód k provedení protokolu v pořadí.</span><span class="sxs-lookup"><span data-stu-id="0a00f-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="0a00f-243">Pokud má uživatel RememberMe je nastavena na místní soubor cookie uživatele, `SignInManager` vrátí `SignInStatus.Success` a nebude muset projít 2FA.</span><span class="sxs-lookup"><span data-stu-id="0a00f-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="0a00f-244">Následující kód ukazuje `SendCode` metody akce.</span><span class="sxs-lookup"><span data-stu-id="0a00f-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="0a00f-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) se vytvoří s všechny metody 2FA povolen pro daného uživatele.</span><span class="sxs-lookup"><span data-stu-id="0a00f-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="0a00f-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) je předán [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocné rutiny, která umožňuje uživateli vybrat přístup 2FA (obvykle e-mail nebo SMS).</span><span class="sxs-lookup"><span data-stu-id="0a00f-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="0a00f-247">Jakmile uživatel odešle přístup 2FA `HTTP POST SendCode` volání metody `SignInManager` odešle kód 2FA a uživatel se přesměruje na `VerifyCode` metody akce, ve kterém můžete zadat kód pro dokončení přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="0a00f-248">2FA uzamčení</span><span class="sxs-lookup"><span data-stu-id="0a00f-248">2FA Lockout</span></span>

<span data-ttu-id="0a00f-249">I když uzamčení účtu lze nastavit na selhání pokusů o přihlášení hesel, tento přístup umožňuje vaše přihlašovací údaje náchylné k [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) uzamčení.</span><span class="sxs-lookup"><span data-stu-id="0a00f-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="0a00f-250">Doporučujeme že použít uzamčení účtu jenom pomocí 2FA.</span><span class="sxs-lookup"><span data-stu-id="0a00f-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="0a00f-251">Když `ApplicationUserManager` je vytvořen, ukázkový kód nastaví uzamčení 2FA a `MaxFailedAccessAttemptsBeforeLockout` na pět.</span><span class="sxs-lookup"><span data-stu-id="0a00f-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="0a00f-252">Jakmile se uživatel přihlásí (prostřednictvím místního účtu nebo účtu na sociální síti), ukládají jednotlivé neúspěšné pokusy o na 2FA a při dosažení maximální počet pokusů o je uživatel uzamčen po dobu pěti minut (můžete nastavit zámek čas `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="0a00f-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="0a00f-253">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="0a00f-253">Additional Resources</span></span>

- <span data-ttu-id="0a00f-254">[ASP.NET Identity – doporučené prostředky](../getting-started/aspnet-identity-recommended-resources.md) úplný seznam identit blogy, videa, kurzy a velké tak odkazy.</span><span class="sxs-lookup"><span data-stu-id="0a00f-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="0a00f-255">[Aplikace MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) také ukazuje, jak přidat informace o profilu do tabulky uživatelů.</span><span class="sxs-lookup"><span data-stu-id="0a00f-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="0a00f-256">[ASP.NET MVC a Identity 2.0: Základy práce s](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) podle Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="0a00f-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="0a00f-257">Potvrzení účtu a obnovení hesla s ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="0a00f-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="0a00f-258">Úvod do ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="0a00f-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="0a00f-259">[Oznamujeme vydání verze RTM technologie ASP.NET identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) napsal Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="0a00f-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="0a00f-260">[Identita ASP.NET 2.0: Nastavení ověření účtu a povolení dvoufaktorového](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) podle Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="0a00f-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
