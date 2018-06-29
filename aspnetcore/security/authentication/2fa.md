---
title: Dvoufaktorové ověřování pomocí SMS v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak nastavit dvoufaktorové ověřování (2FA) s aplikací ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 0308b05ebcda1af7f6850549d7a33f1df1a912a0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089981"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="4f415-103">Dvoufaktorové ověřování pomocí SMS v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f415-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="4f415-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [mezi Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="4f415-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

 <span data-ttu-id="4f415-105">Dva faktor ověřování (2FA), použití založené na čase jednorázové heslo algoritmus (TOTP), jsou tyto aplikace doporučenému přístupu pro 2FA odvětví.</span><span class="sxs-lookup"><span data-stu-id="4f415-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="4f415-106">2FA pomocí TOTP je upřednostňovaný k 2FA serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="4f415-107">Další informace najdete v tématu [generování povolit kód QR pro TOTP aplikace v ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pro technologii ASP.NET 2.0 jádra a novější.</span><span class="sxs-lookup"><span data-stu-id="4f415-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="4f415-108">Tento kurz ukazuje, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="4f415-109">Jsou uvedeny pokyny pro [twilio](https://www.twilio.com/) a [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale můžete použít další poskytovatele serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="4f415-110">Doporučujeme, abyste dokončení [potvrzení účtu a heslo pro obnovení](xref:security/authentication/accconfirm) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4f415-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="4f415-111">Zobrazení [hotová ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="4f415-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="4f415-112">[Stažení](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="4f415-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="4f415-113">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f415-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="4f415-114">Vytvořit novou webovou aplikaci ASP.NET Core s názvem `Web2FA` s jednotlivých uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="4f415-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="4f415-115">Postupujte podle pokynů v [vynutit SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) nastavit a vyžadovat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="4f415-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="4f415-116">Vytvoření účtu služby SMS</span><span class="sxs-lookup"><span data-stu-id="4f415-116">Create an SMS account</span></span>

<span data-ttu-id="4f415-117">Vytvoření účtu služby SMS, například z [twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="4f415-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="4f415-118">Zaznamenejte ověřovací pověření (pro twilio: accountSid a ověřovacího tokenu pro ASPSMS: Userkey a heslo).</span><span class="sxs-lookup"><span data-stu-id="4f415-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="4f415-119">Zjištění přihlašovací údaje poskytovatele služby SMS</span><span class="sxs-lookup"><span data-stu-id="4f415-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="4f415-120">**Twilio:** z karty řídicí panel svého účtu Twilio, zkopírujte **SID účtu** a **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="4f415-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="4f415-121">**ASPSMS:** v nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej společně s vaší **heslo**.</span><span class="sxs-lookup"><span data-stu-id="4f415-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="4f415-122">Později jsme uloží tyto hodnoty pomocí nástroje Správce tajný klíč v rámci klíče `SMSAccountIdentification` a `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="4f415-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="4f415-123">Zadání ID odesílatele nebo původce</span><span class="sxs-lookup"><span data-stu-id="4f415-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="4f415-124">**Twilio:** z čísla karty, zkopírujte vaše Twilio **telefonní číslo**.</span><span class="sxs-lookup"><span data-stu-id="4f415-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="4f415-125">**ASPSMS:** v nabídce autoru odemknutí odemknutí jeden nebo více autoru nebo vyberte alfanumerické původce (nepodporuje všechny sítě).</span><span class="sxs-lookup"><span data-stu-id="4f415-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="4f415-126">Později jsme uloží tuto hodnotu pomocí nástroje Správce tajný klíč v rámci klíč `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="4f415-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="4f415-127">Zadejte přihlašovací údaje pro službu SMS</span><span class="sxs-lookup"><span data-stu-id="4f415-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="4f415-128">Použijeme [možnosti vzor](xref:fundamentals/configuration/options) pro přístup k účtu a klíč nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="4f415-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="4f415-129">Vytvořte třídu načíst zabezpečený klíč serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="4f415-130">Tato ukázka `SMSoptions` je v vytvořit třídu *Services/SMSoptions.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="4f415-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="4f415-131">Nastavte `SMSAccountIdentification`, `SMSAccountPassword` a `SMSAccountFrom` s [nástroj tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="4f415-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="4f415-132">Příklad:</span><span class="sxs-lookup"><span data-stu-id="4f415-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="4f415-133">Přidejte balíček NuGet pro poskytovatele služby SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="4f415-134">Z balíček správce konzoly (pomocí PMC) spusťte:</span><span class="sxs-lookup"><span data-stu-id="4f415-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="4f415-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="4f415-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="4f415-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="4f415-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="4f415-137">Přidejte kód v *Services/MessageServices.cs* souboru SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="4f415-138">Použijte Twilio nebo ASPSMS části:</span><span class="sxs-lookup"><span data-stu-id="4f415-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="4f415-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="4f415-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="4f415-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="4f415-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="4f415-141">Konfigurace spuštění používat `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="4f415-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="4f415-142">Přidat `SMSoptions` ke kontejneru služby v `ConfigureServices` metoda v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4f415-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="4f415-143">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="4f415-143">Enable two-factor authentication</span></span>

<span data-ttu-id="4f415-144">Otevřete *Views/Manage/Index.cshtml* souboru nástroje Razor zobrazení a odebrání komentář znaků (takže žádný kód je odkomentovaný).</span><span class="sxs-lookup"><span data-stu-id="4f415-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="4f415-145">Přihlaste se pomocí dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="4f415-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="4f415-146">Spusťte aplikaci a zaregistrovat nového uživatele</span><span class="sxs-lookup"><span data-stu-id="4f415-146">Run the app and register a new user</span></span>

![Webovou aplikaci zaregistrovat zobrazení otevřít v Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="4f415-148">Klepněte na jméno uživatele, který aktivuje `Index` metodu akce v kontroleru spravovat.</span><span class="sxs-lookup"><span data-stu-id="4f415-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="4f415-149">Potom klepněte na telefonní číslo **přidat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="4f415-149">Then tap the phone number **Add** link.</span></span>

![Správa zobrazení](2fa/_static/login2fa2.png)

* <span data-ttu-id="4f415-151">Telefonní číslo, který bude přijímat ověřovací kód a klepněte na Přidat **Odeslat ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="4f415-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Přidat stránku telefonní číslo](2fa/_static/login2fa3.png)

* <span data-ttu-id="4f415-153">Zobrazí se zpráva SMS s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="4f415-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="4f415-154">Zadejte ji a klepněte na **odeslání**</span><span class="sxs-lookup"><span data-stu-id="4f415-154">Enter it and tap **Submit**</span></span>

![Zkontrolujte telefonní číslo stránky](2fa/_static/login2fa4.png)

<span data-ttu-id="4f415-156">Pokud neobdržíte textovou zprávu, najdete v protokolu stránky twilio.</span><span class="sxs-lookup"><span data-stu-id="4f415-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="4f415-157">Správa zobrazení ukazuje, že váš telefon byl úspěšně přidán.</span><span class="sxs-lookup"><span data-stu-id="4f415-157">The Manage view shows your phone number was added successfully.</span></span>

![Správa zobrazení](2fa/_static/login2fa5.png)

* <span data-ttu-id="4f415-159">Klepněte na **povolit** povolit dvoufaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="4f415-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Správa zobrazení](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="4f415-161">Test dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="4f415-161">Test two-factor authentication</span></span>

* <span data-ttu-id="4f415-162">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="4f415-162">Log off.</span></span>

* <span data-ttu-id="4f415-163">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="4f415-163">Log in.</span></span>

* <span data-ttu-id="4f415-164">Uživatelský účet má povoleno dvoufaktorové ověřování, je nutné zadat druhý faktor ověřování.</span><span class="sxs-lookup"><span data-stu-id="4f415-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="4f415-165">V tomto kurzu jste povolili ověření telefonu.</span><span class="sxs-lookup"><span data-stu-id="4f415-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="4f415-166">Integrované šablony lze také nastavit e-mail. jako druhý faktor.</span><span class="sxs-lookup"><span data-stu-id="4f415-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="4f415-167">Můžete nastavit další druhý faktory ověřování, jako je kódy QR.</span><span class="sxs-lookup"><span data-stu-id="4f415-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="4f415-168">Klepněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="4f415-168">Tap **Submit**.</span></span>

![Odeslat ověřovací kód zobrazení](2fa/_static/login2fa7.png)

* <span data-ttu-id="4f415-170">Zadejte kód, který se zobrazí ve zprávě SMS.</span><span class="sxs-lookup"><span data-stu-id="4f415-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="4f415-171">Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení při použití stejné zařízení a prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4f415-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="4f415-172">Povolení 2FA a kliknete na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="4f415-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="4f415-173">Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="4f415-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="4f415-174">Nastavením **mějte na paměti, tento prohlížeč**, získáte další zabezpečení 2FA ze zařízení, nepoužívejte pravidelně a získat užitečný na nebudou muset projít 2FA na vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="4f415-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Ověření zobrazení](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="4f415-176">Uzamčení účtu pro ochranu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="4f415-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="4f415-177">Uzamčení účtu se doporučuje s 2FA.</span><span class="sxs-lookup"><span data-stu-id="4f415-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="4f415-178">Jakmile se uživatel přihlásí pomocí místního účtu nebo sociální účtu, je uložený v 2FA každý neúspěšný pokus.</span><span class="sxs-lookup"><span data-stu-id="4f415-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="4f415-179">Pokud je dostupný maximální neúspěšných pokusů o přístup, je uživatel uzamčen (výchozí: 5 minut uzamčení po 5 neúspěšných pokusů o přístup).</span><span class="sxs-lookup"><span data-stu-id="4f415-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="4f415-180">Úspěšné ověření resetuje počet pokusů o neúspěšných přístupů a obnoví hodiny.</span><span class="sxs-lookup"><span data-stu-id="4f415-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="4f415-181">Maximální počet neúspěšných pokusů o přístup a doby uzamčení lze nastavit s [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) a [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="4f415-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="4f415-182">Následující konfiguruje uzamčení účtu po dobu 10 minut po 10 neúspěšných pokusů o přístup:</span><span class="sxs-lookup"><span data-stu-id="4f415-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="4f415-183">Potvrďte, že [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) nastaví `lockoutOnFailure` k `true`:</span><span class="sxs-lookup"><span data-stu-id="4f415-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
