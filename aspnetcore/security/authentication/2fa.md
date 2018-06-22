---
title: Dvoufaktorové ověřování pomocí SMS v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak nastavit dvoufaktorové ověřování (2FA) s aplikací ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 335edfd5cd4dfbb9d223ba0ae888a6d2386cd4a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272306"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="7ebae-103">Dvoufaktorové ověřování pomocí SMS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ebae-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="7ebae-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [mezi Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="7ebae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="7ebae-105">V tématu [vygenerovat kód QR povolit pro aplikace v ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pro technologii ASP.NET 2.0 jádra a novější.</span><span class="sxs-lookup"><span data-stu-id="7ebae-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="7ebae-106">Tento kurz ukazuje, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="7ebae-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="7ebae-107">Jsou uvedeny pokyny pro [twilio](https://www.twilio.com/) a [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale můžete použít další poskytovatele serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="7ebae-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="7ebae-108">Doporučujeme, abyste dokončení [potvrzení účtu a heslo pro obnovení](xref:security/authentication/accconfirm) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7ebae-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="7ebae-109">Zobrazení [hotová ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="7ebae-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="7ebae-110">[Stažení](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="7ebae-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="7ebae-111">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ebae-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="7ebae-112">Vytvořit novou webovou aplikaci ASP.NET Core s názvem `Web2FA` s jednotlivých uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="7ebae-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="7ebae-113">Postupujte podle pokynů v [vynutit SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) nastavit a vyžadovat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="7ebae-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="7ebae-114">Vytvoření účtu služby SMS</span><span class="sxs-lookup"><span data-stu-id="7ebae-114">Create an SMS account</span></span>

<span data-ttu-id="7ebae-115">Vytvoření účtu služby SMS, například z [twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="7ebae-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="7ebae-116">Zaznamenejte ověřovací pověření (pro twilio: accountSid a ověřovacího tokenu pro ASPSMS: Userkey a heslo).</span><span class="sxs-lookup"><span data-stu-id="7ebae-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="7ebae-117">Zjištění přihlašovací údaje poskytovatele služby SMS</span><span class="sxs-lookup"><span data-stu-id="7ebae-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="7ebae-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-118">**Twilio:**</span></span>  
<span data-ttu-id="7ebae-119">Na kartě řídicí panel svého účtu Twilio zkopírovat **SID účtu** a **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="7ebae-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="7ebae-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-120">**ASPSMS:**</span></span>  
<span data-ttu-id="7ebae-121">V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej společně s vaší **heslo**.</span><span class="sxs-lookup"><span data-stu-id="7ebae-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="7ebae-122">Později jsme uloží tyto hodnoty pomocí nástroje Správce tajný klíč v rámci klíče `SMSAccountIdentification` a `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="7ebae-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="7ebae-123">Zadání ID odesílatele nebo původce</span><span class="sxs-lookup"><span data-stu-id="7ebae-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="7ebae-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-124">**Twilio:**</span></span>  
<span data-ttu-id="7ebae-125">Na kartě čísla zkopírujte vaše Twilio **telefonní číslo**.</span><span class="sxs-lookup"><span data-stu-id="7ebae-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="7ebae-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-126">**ASPSMS:**</span></span>  
<span data-ttu-id="7ebae-127">V nabídce autoru odemknutí odemknutí jeden nebo více autoru nebo zvolte alfanumerické původce (nepodporuje všechny sítě).</span><span class="sxs-lookup"><span data-stu-id="7ebae-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="7ebae-128">Později jsme uloží tuto hodnotu pomocí nástroje Správce tajný klíč v rámci klíč `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="7ebae-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="7ebae-129">Zadejte přihlašovací údaje pro službu SMS</span><span class="sxs-lookup"><span data-stu-id="7ebae-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="7ebae-130">Použijeme [možnosti vzor](xref:fundamentals/configuration/options) pro přístup k účtu a klíč nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="7ebae-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="7ebae-131">Vytvořte třídu načíst zabezpečený klíč serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="7ebae-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="7ebae-132">Tato ukázka `SMSoptions` je v vytvořit třídu *Services/SMSoptions.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="7ebae-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="7ebae-133">Nastavte `SMSAccountIdentification`, `SMSAccountPassword` a `SMSAccountFrom` s [nástroj tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7ebae-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7ebae-134">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7ebae-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="7ebae-135">Přidejte balíček NuGet pro poskytovatele služby SMS.</span><span class="sxs-lookup"><span data-stu-id="7ebae-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="7ebae-136">Z balíček správce konzoly (pomocí PMC) spusťte:</span><span class="sxs-lookup"><span data-stu-id="7ebae-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="7ebae-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="7ebae-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="7ebae-139">Přidejte kód v *Services/MessageServices.cs* souboru SMS.</span><span class="sxs-lookup"><span data-stu-id="7ebae-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="7ebae-140">Použijte Twilio nebo ASPSMS části:</span><span class="sxs-lookup"><span data-stu-id="7ebae-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="7ebae-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="7ebae-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="7ebae-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="7ebae-143">Konfigurace spuštění používat `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="7ebae-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="7ebae-144">Přidat `SMSoptions` ke kontejneru služby v `ConfigureServices` metoda v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ebae-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="7ebae-145">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="7ebae-145">Enable two-factor authentication</span></span>

<span data-ttu-id="7ebae-146">Otevřete *Views/Manage/Index.cshtml* souboru nástroje Razor zobrazení a odebrání komentář znaků (takže žádný kód je odkomentovaný).</span><span class="sxs-lookup"><span data-stu-id="7ebae-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="7ebae-147">Přihlaste se pomocí dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="7ebae-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="7ebae-148">Spusťte aplikaci a zaregistrovat nového uživatele</span><span class="sxs-lookup"><span data-stu-id="7ebae-148">Run the app and register a new user</span></span>

![Webovou aplikaci zaregistrovat zobrazení otevřít v Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="7ebae-150">Klepněte na jméno uživatele, který aktivuje `Index` metodu akce v kontroleru spravovat.</span><span class="sxs-lookup"><span data-stu-id="7ebae-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="7ebae-151">Potom klepněte na telefonní číslo **přidat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="7ebae-151">Then tap the phone number **Add** link.</span></span>

![Správa zobrazení](2fa/_static/login2fa2.png)

* <span data-ttu-id="7ebae-153">Telefonní číslo, který bude přijímat ověřovací kód a klepněte na Přidat **Odeslat ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="7ebae-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Přidat stránku telefonní číslo](2fa/_static/login2fa3.png)

* <span data-ttu-id="7ebae-155">Zobrazí se zpráva SMS s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="7ebae-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="7ebae-156">Zadejte ji a klepněte na **odeslání**</span><span class="sxs-lookup"><span data-stu-id="7ebae-156">Enter it and tap **Submit**</span></span>

![Zkontrolujte telefonní číslo stránky](2fa/_static/login2fa4.png)

<span data-ttu-id="7ebae-158">Pokud neobdržíte textovou zprávu, najdete v protokolu stránky twilio.</span><span class="sxs-lookup"><span data-stu-id="7ebae-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="7ebae-159">Správa zobrazení ukazuje, že váš telefon byl úspěšně přidán.</span><span class="sxs-lookup"><span data-stu-id="7ebae-159">The Manage view shows your phone number was added successfully.</span></span>

![Správa zobrazení](2fa/_static/login2fa5.png)

* <span data-ttu-id="7ebae-161">Klepněte na **povolit** povolit dvoufaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="7ebae-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Správa zobrazení](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="7ebae-163">Test dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="7ebae-163">Test two-factor authentication</span></span>

* <span data-ttu-id="7ebae-164">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="7ebae-164">Log off.</span></span>

* <span data-ttu-id="7ebae-165">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="7ebae-165">Log in.</span></span>

* <span data-ttu-id="7ebae-166">Uživatelský účet má povoleno dvoufaktorové ověřování, je nutné zadat druhý faktor ověřování.</span><span class="sxs-lookup"><span data-stu-id="7ebae-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="7ebae-167">V tomto kurzu jste povolili ověření telefonu.</span><span class="sxs-lookup"><span data-stu-id="7ebae-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="7ebae-168">Integrované šablony lze také nastavit e-mail. jako druhý faktor.</span><span class="sxs-lookup"><span data-stu-id="7ebae-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="7ebae-169">Můžete nastavit další druhý faktory ověřování, jako je kódy QR.</span><span class="sxs-lookup"><span data-stu-id="7ebae-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="7ebae-170">Klepněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="7ebae-170">Tap **Submit**.</span></span>

![Odeslat ověřovací kód zobrazení](2fa/_static/login2fa7.png)

* <span data-ttu-id="7ebae-172">Zadejte kód, který se zobrazí ve zprávě SMS.</span><span class="sxs-lookup"><span data-stu-id="7ebae-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="7ebae-173">Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení při použití stejné zařízení a prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="7ebae-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="7ebae-174">Povolení 2FA a kliknete na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ebae-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="7ebae-175">Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="7ebae-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="7ebae-176">Nastavením **mějte na paměti, tento prohlížeč**, získáte další zabezpečení 2FA ze zařízení, nepoužívejte pravidelně a získat užitečný na nebudou muset projít 2FA na vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="7ebae-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Ověření zobrazení](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="7ebae-178">Uzamčení účtu pro ochranu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="7ebae-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="7ebae-179">Uzamčení účtu se doporučuje s 2FA.</span><span class="sxs-lookup"><span data-stu-id="7ebae-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="7ebae-180">Jakmile se uživatel přihlásí pomocí místního účtu nebo sociální účtu, je uložený v 2FA každý neúspěšný pokus.</span><span class="sxs-lookup"><span data-stu-id="7ebae-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="7ebae-181">Pokud je dostupný maximální neúspěšných pokusů o přístup, je uživatel uzamčen (výchozí: 5 minut uzamčení po 5 neúspěšných pokusů o přístup).</span><span class="sxs-lookup"><span data-stu-id="7ebae-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="7ebae-182">Úspěšné ověření resetuje počet pokusů o neúspěšných přístupů a obnoví hodiny.</span><span class="sxs-lookup"><span data-stu-id="7ebae-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="7ebae-183">Maximální počet neúspěšných pokusů o přístup a doby uzamčení lze nastavit s [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) a [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="7ebae-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="7ebae-184">Následující konfiguruje uzamčení účtu po dobu 10 minut po 10 neúspěšných pokusů o přístup:</span><span class="sxs-lookup"><span data-stu-id="7ebae-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="7ebae-185">Potvrďte, že [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) nastaví `lockoutOnFailure` k `true`:</span><span class="sxs-lookup"><span data-stu-id="7ebae-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
