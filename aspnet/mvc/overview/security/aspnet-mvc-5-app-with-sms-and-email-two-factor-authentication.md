---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikace ASP.NET MVC 5 s SMS a e-mailu Dvojúrovňového ověřování | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5 s dvoufaktorovým ověřováním. Vytvoření zabezpečené ASP.NET MVC 5 webové aplikace s by se měla dokončit...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: a422988f681273b153725d32bb8337deb25b12f1
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912589"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="6a018-104">Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="6a018-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="6a018-105">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="6a018-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="6a018-106">V tomto kurzu se dozvíte, jak vytvořit webovou aplikaci ASP.NET MVC 5 s dvoufaktorovým ověřováním.</span><span class="sxs-lookup"><span data-stu-id="6a018-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="6a018-107">By se měla Dokončit [vytvořit zabezpečenou webovou aplikaci ASP.NET MVC 5 s přihlášením, resetovat heslo a potvrzení e-mailu](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6a018-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="6a018-108">Můžete stáhnout hotovou aplikaci [tady](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="6a018-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="6a018-109">Položka ke stažení obsahuje ladění pomocné rutiny, které umožňují otestovat bez nastavení e-mailem nebo poskytovatele služby SMS serveru SMS a e-mailové potvrzení.</span><span class="sxs-lookup"><span data-stu-id="6a018-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="6a018-110">V tomto kurzu zapsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="6a018-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="6a018-111">Vytvoření aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6a018-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="6a018-112">Nastavení služby SMS pro dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="6a018-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="6a018-113">Povolení dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="6a018-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="6a018-114">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6a018-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="6a018-115">Vytvoření aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6a018-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="6a018-116">Začněte tím, že instalaci a používání [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="6a018-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="6a018-117">Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="6a018-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="6a018-118">Upozornění: By se měla Dokončit [vytvořit zabezpečenou webovou aplikaci ASP.NET MVC 5 s přihlášením, resetovat heslo a potvrzení e-mailu](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="6a018-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="6a018-119">Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší, k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6a018-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="6a018-120">Vytvoření nového projektu ASP.NET Web a vyberte šablonu MVC.</span><span class="sxs-lookup"><span data-stu-id="6a018-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="6a018-121">Webové formuláře také podporuje ASP.NET Identity, takže může podle podobných kroků ve webové aplikaci formulářů.</span><span class="sxs-lookup"><span data-stu-id="6a018-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="6a018-122">Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="6a018-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="6a018-123">Pokud chcete hostovat aplikace ve službě Azure, ponechte políčko zaškrtnuté.</span><span class="sxs-lookup"><span data-stu-id="6a018-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="6a018-124">Později v tomto kurzu nasadíme do Azure.</span><span class="sxs-lookup"><span data-stu-id="6a018-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="6a018-125">Je možné [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="6a018-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="6a018-126">Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="6a018-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="6a018-127">Nastavení služby SMS pro dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="6a018-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="6a018-128">Tento kurz obsahuje pokyny k používání Twilio nebo ASPSMS ale můžete použít jakýkoli jiný poskytovatel serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="6a018-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="6a018-129">**Vytvoření uživatelského účtu s poskytovatelem služby SMS**</span><span class="sxs-lookup"><span data-stu-id="6a018-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="6a018-130">Vytvoření [Twilio](https://www.twilio.com/try-twilio) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) účtu.</span><span class="sxs-lookup"><span data-stu-id="6a018-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="6a018-131">**Instalace dalších balíčků nebo přidávání odkazů na služby**</span><span class="sxs-lookup"><span data-stu-id="6a018-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="6a018-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="6a018-132">Twilio:</span></span>  
   <span data-ttu-id="6a018-133">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6a018-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="6a018-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="6a018-134">ASPSMS:</span></span>  
   <span data-ttu-id="6a018-135">Následující odkaz na službu musí být přidán:</span><span class="sxs-lookup"><span data-stu-id="6a018-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="6a018-136">Adresa:</span><span class="sxs-lookup"><span data-stu-id="6a018-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="6a018-137">Obor názvů:</span><span class="sxs-lookup"><span data-stu-id="6a018-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="6a018-138">**Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**</span><span class="sxs-lookup"><span data-stu-id="6a018-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="6a018-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="6a018-139">Twilio:</span></span>  
   <span data-ttu-id="6a018-140">Z **řídicí panel** kartu svého účtu Twilio, kopie **SID účtu** a **ověřovací token**.</span><span class="sxs-lookup"><span data-stu-id="6a018-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="6a018-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="6a018-141">ASPSMS:</span></span>  
   <span data-ttu-id="6a018-142">V nastavení účtu, přejděte na **vlastnosti userkey jedná** a zkopírujte ho spolu s místním definované **heslo**.</span><span class="sxs-lookup"><span data-stu-id="6a018-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="6a018-143">Později jsme uloží tyto hodnoty *web.config* souboru v rámci klíče `"SMSAccountIdentification"` a `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="6a018-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="6a018-144">**Určení SenderID / původce**</span><span class="sxs-lookup"><span data-stu-id="6a018-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="6a018-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="6a018-145">Twilio:</span></span>  
   <span data-ttu-id="6a018-146">Z **čísla** kartu, zkopírujte své telefonní číslo Twilio.</span><span class="sxs-lookup"><span data-stu-id="6a018-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="6a018-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="6a018-147">ASPSMS:</span></span>  
   <span data-ttu-id="6a018-148">V rámci **odemknout autoru** nabídce odemknout jeden nebo více autoru nebo zvolte příkazce alfanumerické znaky (není podporováno ve všech sítích).</span><span class="sxs-lookup"><span data-stu-id="6a018-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="6a018-149">Dále jsme uloží tuto hodnotu v *web.config* soubor daný klíč `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="6a018-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="6a018-150">**Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**</span><span class="sxs-lookup"><span data-stu-id="6a018-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="6a018-151">Přihlašovací údaje a zpřístupníte odesílatele telefonní číslo do aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a018-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="6a018-152">Pro zjednodušení jsme uloží tyto hodnoty *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="6a018-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="6a018-153">Pokud nasadíme do Azure, abychom mohli uložit hodnoty bezpečně v **nastavení aplikace** karta Konfigurace části na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="6a018-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="6a018-154">Zabezpečení – nikdy ukládání citlivých dat ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="6a018-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="6a018-155">Účet a přihlašovací údaje se přidají do výše uvedený pro zjednodušení ukázkový kód.</span><span class="sxs-lookup"><span data-stu-id="6a018-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="6a018-156">Zobrazit [osvědčené postupy pro nasazení hesel a dalších citlivých dat do ASP.NET a Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="6a018-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="6a018-157">**Provádění přenos dat do poskytovatele služby SMS**</span><span class="sxs-lookup"><span data-stu-id="6a018-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="6a018-158">Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="6a018-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="6a018-159">V závislosti na poskytovateli serveru SMS používá aktivaci buď **Twilio** nebo **ASPSMS** části:</span><span class="sxs-lookup"><span data-stu-id="6a018-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="6a018-160">Aktualizace *Views\Manage\Index.cshtml* zobrazení Razor: (Poznámka: není právě komentářů ve stávající kód, použijte následující kód.)</span><span class="sxs-lookup"><span data-stu-id="6a018-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="6a018-161">Ověření `EnableTwoFactorAuthentication` a `DisableTwoFactorAuthentication` metody akce v `ManageController` mít[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atribut:</span><span class="sxs-lookup"><span data-stu-id="6a018-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="6a018-162">Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve zaregistrovali.</span><span class="sxs-lookup"><span data-stu-id="6a018-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="6a018-163">Klikněte na své ID uživatele, která se aktivuje `Index` metodu akce v `Manage` kontroleru.</span><span class="sxs-lookup"><span data-stu-id="6a018-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="6a018-164">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="6a018-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="6a018-165">`AddPhoneNumber` Metoda akce se zobrazí dialogové okno zadat telefonní číslo, které můžou přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="6a018-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="6a018-166">Během několika sekund obdržíte textovou zprávu s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="6a018-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="6a018-167">Zadejte ji a stiskněte klávesu **odeslat**.</span><span class="sxs-lookup"><span data-stu-id="6a018-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="6a018-168">Správa zobrazení ukazuje že vaše telefonní číslo bylo přidáno.</span><span class="sxs-lookup"><span data-stu-id="6a018-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="6a018-169">Povolení dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="6a018-169">Enable two-factor authentication</span></span>

<span data-ttu-id="6a018-170">V aplikaci vygenerovanou šablonu budete muset použít uživatelské rozhraní pro povolení dvoufaktorového ověřování (2FA).</span><span class="sxs-lookup"><span data-stu-id="6a018-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="6a018-171">Pokud chcete povolit 2FA, klikněte na své ID uživatele (e-mailový alias) na navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="6a018-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="6a018-172">Klikněte na povolit 2FA.</span><span class="sxs-lookup"><span data-stu-id="6a018-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="6a018-173">Odhlášení, potom znovu přihlásit v.</span><span class="sxs-lookup"><span data-stu-id="6a018-173">Logout, then log back in.</span></span> <span data-ttu-id="6a018-174">Pokud jste povolili e-mailu (naleznete v tématu Moje [předchozí kurz o službě](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat, SMS nebo e-mailu pro 2FA.</span><span class="sxs-lookup"><span data-stu-id="6a018-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="6a018-175">Zobrazí se stránka Ověřte kód, ve kterém můžete zadat kód (z SMS nebo e-mailu).</span><span class="sxs-lookup"><span data-stu-id="6a018-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="6a018-176">Kliknutím na **chcete Zapamatovat tento prohlížeč** zaškrtávacího políčka můžete vyloučit z by bylo potřeba při používání prohlížeče a zařízení, kde zaškrtnuté políčko přihlášení pomocí 2FA.</span><span class="sxs-lookup"><span data-stu-id="6a018-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="6a018-177">Za předpokladu, uživateli se zlými úmysly nemůže získat přístup k zařízení, povolení 2FA a kliknete na **chcete Zapamatovat tento prohlížeč** vám poskytne pohodlný přístup heslo jeden krok, při zachování ochrany silné 2FA pro veškerý přístup z nedůvěryhodné zařízení.</span><span class="sxs-lookup"><span data-stu-id="6a018-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="6a018-178">Můžete to provést na libovolném privátní zařízení, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="6a018-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="6a018-179">Tento kurz obsahuje rychlý úvod k povolení 2FA na novou aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6a018-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="6a018-180">Tento kurz [dvoufaktorovým ověřováním pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) obsahuje podrobnosti na pozadí ukázky kódu.</span><span class="sxs-lookup"><span data-stu-id="6a018-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="6a018-181">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6a018-181">Additional Resources</span></span>

- <span data-ttu-id="6a018-182">[Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) obsahuje podrobnosti o dvoufaktorového ověřování</span><span class="sxs-lookup"><span data-stu-id="6a018-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="6a018-183">Odkazy na ASP.NET Identity doporučené zdroje informací</span><span class="sxs-lookup"><span data-stu-id="6a018-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="6a018-184">[Potvrzení účtu a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) obsahuje větší podrobnosti o potvrzení hesla obnovení a účtu.</span><span class="sxs-lookup"><span data-stu-id="6a018-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="6a018-185">[Aplikace MVC 5 s Facebook, Twitter, LinkedIn a Google OAuth2 přihlašování](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) v tomto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s autorizací Facebook nebo Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="6a018-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="6a018-186">Také ukazuje, jak přidat další data do databáze nástroje Identity.</span><span class="sxs-lookup"><span data-stu-id="6a018-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="6a018-187">[Nasazení zabezpečenou aplikaci ASP.NET MVC pomocí členství, OAuth a SQL Database do Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="6a018-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="6a018-188">V tomto kurzu přidá nasazení v Azure, jak zabezpečit vaši aplikaci s rolemi, jak můžete členské rozhraní API přidávat uživatele a role a další funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="6a018-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="6a018-189">Vytvoření aplikace služby Google OAuth 2 a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="6a018-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="6a018-190">Vytvoření aplikace na Facebooku a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="6a018-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="6a018-191">Nastavení protokolu SSL v projektu</span><span class="sxs-lookup"><span data-stu-id="6a018-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="6a018-192">Jak nastavit vývojové prostředí jazyka C# a architektura ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="6a018-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
