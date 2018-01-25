---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování | Microsoft Docs"
author: Rick-Anderson
description: "Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s dvoufaktorové ověřování. Vytvoření zabezpečeného ASP.NET MVC 5 webové aplikace s by se měla dokončit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: d6bc92f3cbe6b61332e33e8a507b4516bf5c15a5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="33a38-104">Aplikace ASP.NET MVC 5 s SMS a e-mailu dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="33a38-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="33a38-105">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="33a38-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="33a38-106">Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s dvoufaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="33a38-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="33a38-107">By se měla Dokončit [vytvořit zabezpečený webovou aplikaci ASP.NET MVC 5 s protokolu v e-mailu potvrzení a heslo resetovat](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="33a38-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="33a38-108">Hotová aplikace si můžete stáhnout [zde](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="33a38-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="33a38-109">Stahování obsahuje ladění pomocné rutiny, které umožňují testování potvrzení e-mailu a SMS bez nastavení e-mailem nebo poskytovatele služby SMS.</span><span class="sxs-lookup"><span data-stu-id="33a38-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="33a38-110">V tomto kurzu napsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="33a38-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="33a38-111">Vytvoření aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="33a38-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="33a38-112">Nastavení služby SMS pro dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="33a38-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="33a38-113">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="33a38-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="33a38-114">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="33a38-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="33a38-115">Vytvoření aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="33a38-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="33a38-116">Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="33a38-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="33a38-117">Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="33a38-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="33a38-118">Upozornění: By se měla Dokončit [vytvořit zabezpečený webovou aplikaci ASP.NET MVC 5 s protokolu v e-mailu potvrzení a heslo resetovat](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="33a38-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="33a38-119">Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="33a38-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="33a38-120">Vytvořte nový projekt ASP.NET Web a vyberte šablonu MVC.</span><span class="sxs-lookup"><span data-stu-id="33a38-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="33a38-121">Webové formuláře také podporuje ASP.NET Identity, takže můžete sledovat v aplikaci web forms podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="33a38-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="33a38-122">Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="33a38-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="33a38-123">Pokud chcete hostovat aplikaci v Azure, nechte políčko zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="33a38-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="33a38-124">Později v tomto kurzu jsme nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="33a38-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="33a38-125">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="33a38-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="33a38-126">Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="33a38-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="33a38-127">Nastavení služby SMS pro dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="33a38-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="33a38-128">Tento kurz obsahuje pokyny pro použití Twilio nebo ASPSMS ale můžete použít další poskytovatele serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="33a38-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="33a38-129">**Vytvoření uživatelského účtu s poskytovatelem služby SMS**</span><span class="sxs-lookup"><span data-stu-id="33a38-129">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="33a38-130">Vytvoření [Twilio](https://www.twilio.com/try-twilio) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) účtu.</span><span class="sxs-lookup"><span data-stu-id="33a38-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="33a38-131">**Instalace dalších balíčků nebo přidání odkazů na služby**</span><span class="sxs-lookup"><span data-stu-id="33a38-131">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="33a38-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="33a38-132">Twilio:</span></span>  
 <span data-ttu-id="33a38-133">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="33a38-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="33a38-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="33a38-134">ASPSMS:</span></span>  
 <span data-ttu-id="33a38-135">Následující odkaz na službu musí být přidán:</span><span class="sxs-lookup"><span data-stu-id="33a38-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 <span data-ttu-id="33a38-136">Adresa:</span><span class="sxs-lookup"><span data-stu-id="33a38-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="33a38-137">Obor názvů:</span><span class="sxs-lookup"><span data-stu-id="33a38-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="33a38-138">**Zjištění přihlašovací údaje uživatele poskytovatele serveru SMS**</span><span class="sxs-lookup"><span data-stu-id="33a38-138">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="33a38-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="33a38-139">Twilio:</span></span>  
 <span data-ttu-id="33a38-140">Z **řídicí panel** kartě svého účtu Twilio kopie **SID účtu** a **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="33a38-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="33a38-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="33a38-141">ASPSMS:</span></span>  
 <span data-ttu-id="33a38-142">V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej spolu s samoobslužné definované **heslo**.</span><span class="sxs-lookup"><span data-stu-id="33a38-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="33a38-143">Později jsme se uloží v tyto hodnoty *web.config* souboru v rámci klíče `"SMSAccountIdentification"` a `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="33a38-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="33a38-144">**Zadání ID odesílatele nebo původce**</span><span class="sxs-lookup"><span data-stu-id="33a38-144">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="33a38-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="33a38-145">Twilio:</span></span>  
 <span data-ttu-id="33a38-146">Z **čísla** kartě, zkopírujte své telefonní číslo Twilio.</span><span class="sxs-lookup"><span data-stu-id="33a38-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="33a38-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="33a38-147">ASPSMS:</span></span>  
 <span data-ttu-id="33a38-148">V rámci **odemknutí autoru** nabídce odemknutí jeden nebo více autoru nebo vyberte alfanumerické původce (nepodporuje všechny sítě).</span><span class="sxs-lookup"><span data-stu-id="33a38-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="33a38-149">Později jsme uloží tuto hodnotu v *web.config* souboru daný klíč `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="33a38-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="33a38-150">**Přenos přihlašovací údaje poskytovatele služby SMS do aplikace**</span><span class="sxs-lookup"><span data-stu-id="33a38-150">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="33a38-151">Přihlašovací údaje a zpřístupníte odesílatele telefonní číslo do aplikace.</span><span class="sxs-lookup"><span data-stu-id="33a38-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="33a38-152">Pro zjednodušení jsme se uloží v tyto hodnoty *web.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="33a38-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="33a38-153">Když jsme nasadit do Azure, uložíme můžete bezpečně v hodnoty **nastavení aplikace** karta Konfigurace části na webu.</span><span class="sxs-lookup"><span data-stu-id="33a38-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="33a38-154">Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="33a38-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="33a38-155">Účet a přihlašovací údaje se přidají do výše pro zjednodušení v ukázkovém kódu.</span><span class="sxs-lookup"><span data-stu-id="33a38-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="33a38-156">V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="33a38-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="33a38-157">**Implementace přenos dat do poskytovatele služby SMS**</span><span class="sxs-lookup"><span data-stu-id="33a38-157">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="33a38-158">Konfigurace `SmsService` třídy v *aplikace\_Start\IdentityConfig.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="33a38-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="33a38-159">V závislosti na použité poskytovatele služby SMS aktivovat buď **Twilio** nebo **ASPSMS** části:</span><span class="sxs-lookup"><span data-stu-id="33a38-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="33a38-160">Aktualizace *Views\Manage\Index.cshtml* zobrazení syntaxe Razor: (Poznámka: není právě Odebere komentáře v existujícímu kódu, použijte následující kód.)</span><span class="sxs-lookup"><span data-stu-id="33a38-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="33a38-161">Ověřte `EnableTwoFactorAuthentication` a `DisableTwoFactorAuthentication` metody akce v `ManageController` mít[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atribut:</span><span class="sxs-lookup"><span data-stu-id="33a38-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="33a38-162">Spusťte aplikaci a přihlaste se pomocí účtu, který jste dříve registrován.</span><span class="sxs-lookup"><span data-stu-id="33a38-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="33a38-163">Klikněte na vaše ID uživatele, který aktivuje `Index` metodu akce v `Manage` řadiče.</span><span class="sxs-lookup"><span data-stu-id="33a38-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="33a38-164">Klikněte na tlačítko Přidat.</span><span class="sxs-lookup"><span data-stu-id="33a38-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="33a38-165">`AddPhoneNumber` Metoda akce zobrazí dialogové okno zadat telefonní číslo, které může přijímat zprávy SMS.</span><span class="sxs-lookup"><span data-stu-id="33a38-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="33a38-166">Během pár sekund obdržíte textovou zprávu s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="33a38-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="33a38-167">Zadejte ho a stiskněte klávesu **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="33a38-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="33a38-168">Správa zobrazení ukazuje, že vaše telefonní číslo bylo přidáno.</span><span class="sxs-lookup"><span data-stu-id="33a38-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="33a38-169">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="33a38-169">Enable two-factor authentication</span></span>

<span data-ttu-id="33a38-170">V aplikaci vygenerované šablony budete muset povolit dvoufaktorové ověřování (2FA) pomocí uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="33a38-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="33a38-171">Chcete-li povolit 2FA, klikněte na vaše ID uživatele (e-mailový alias) na navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="33a38-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="33a38-172">Klikněte na povolit 2FA.</span><span class="sxs-lookup"><span data-stu-id="33a38-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="33a38-173">Odhlášení, potom znovu přihlásit v.</span><span class="sxs-lookup"><span data-stu-id="33a38-173">Logout, then log back in.</span></span> <span data-ttu-id="33a38-174">Pokud jste povolili e-mailu (najdete v části Moje [předchozí kurzu](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), můžete vybrat SMS nebo e-mailu pro 2FA.</span><span class="sxs-lookup"><span data-stu-id="33a38-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="33a38-175">Zobrazí se stránka ověření kódu, kde můžete zadat kód (z SMS nebo e-mailu).</span><span class="sxs-lookup"><span data-stu-id="33a38-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="33a38-176">Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z museli používat 2FA k přihlášení při používání prohlížeče a zařízení, kde zaškrtnuté políčko.</span><span class="sxs-lookup"><span data-stu-id="33a38-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="33a38-177">Tak dlouho, dokud uživatelé se zlými úmysly nemůže získat přístup k zařízení, povolení 2FA a kliknutím na **mějte na paměti, tento prohlížeč** vám poskytne přístup heslo pohodlný jeden krok, a zároveň zachovat ochranu silným 2FA pro veškerý přístup. z nedůvěryhodné zařízení.</span><span class="sxs-lookup"><span data-stu-id="33a38-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="33a38-178">Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="33a38-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="33a38-179">V tomto kurzu poskytuje rychlý úvod do povolení 2FA na novou aplikaci ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="33a38-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="33a38-180">Moje kurzu [dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) přejde do podrobností na ukázku kódu.</span><span class="sxs-lookup"><span data-stu-id="33a38-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="33a38-181">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="33a38-181">Additional Resources</span></span>

- <span data-ttu-id="33a38-182">[Dvoufaktorové ověřování pomocí SMS a e-mailu s ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) přejde do podrobnosti o dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="33a38-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="33a38-183">Odkazy na identitě ASP.NET Identity doporučené prostředky</span><span class="sxs-lookup"><span data-stu-id="33a38-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="33a38-184">[Účet potvrzení a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do další podrobnosti o potvrzení hesla obnovení a účet.</span><span class="sxs-lookup"><span data-stu-id="33a38-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="33a38-185">[Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) tohoto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s ověřování sítě Facebook a Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="33a38-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="33a38-186">Také ukazuje, jak přidat další data do databáze Identity.</span><span class="sxs-lookup"><span data-stu-id="33a38-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="33a38-187">[Nasazení aplikace zabezpečené rozhraní ASP.NET MVC s členství, OAuth a databáze SQL Azure web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="33a38-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="33a38-188">V tomto kurzu přidá nasazení Azure, jak zabezpečit vaše aplikace s rolemi, jak používat členské rozhraní API k přidání uživatelů a rolí a další funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="33a38-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="33a38-189">Vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="33a38-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="33a38-190">Vytvoření aplikace v síti Facebook a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="33a38-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="33a38-191">Nastavení protokolu SSL v projektu</span><span class="sxs-lookup"><span data-stu-id="33a38-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
