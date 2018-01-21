---
title: "Dvoufaktorové ověřování pomocí serveru SMS"
author: rick-anderson
description: "Ukazuje, jak nastavit dvoufaktorové ověřování (2FA) s ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 0970b7e95b116ceab1d17502d2f8aee9cd821715
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="03694-103">Dvoufaktorové ověřování pomocí serveru SMS</span><span class="sxs-lookup"><span data-stu-id="03694-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="03694-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [mezi Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="03694-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="03694-105">V tomto kurzu platí pro ASP.NET Core pouze 1.x.</span><span class="sxs-lookup"><span data-stu-id="03694-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="03694-106">V tématu [vygenerovat kód QR povolení pro aplikace v ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pro technologii ASP.NET 2.0 jádra a novější.</span><span class="sxs-lookup"><span data-stu-id="03694-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="03694-107">Tento kurz ukazuje, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="03694-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="03694-108">Jsou uvedeny pokyny pro [twilio](https://www.twilio.com/) a [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale můžete použít další poskytovatele serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="03694-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="03694-109">Doporučujeme, abyste dokončení [potvrzení účtu a heslo pro obnovení](accconfirm.md) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="03694-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="03694-110">Zobrazení [hotová ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="03694-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="03694-111">[Stažení](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="03694-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="03694-112">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03694-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="03694-113">Vytvořit novou webovou aplikaci ASP.NET Core s názvem `Web2FA` s jednotlivých uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="03694-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="03694-114">Postupujte podle pokynů v [vynucování SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) nastavit a vyžadovat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="03694-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="03694-115">Vytvoření účtu služby SMS</span><span class="sxs-lookup"><span data-stu-id="03694-115">Create an SMS account</span></span>

<span data-ttu-id="03694-116">Vytvoření účtu služby SMS, například z [twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="03694-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="03694-117">Zaznamenejte ověřovací pověření (pro twilio: accountSid a ověřovacího tokenu pro ASPSMS: Userkey a heslo).</span><span class="sxs-lookup"><span data-stu-id="03694-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="03694-118">Zjištění přihlašovací údaje poskytovatele služby SMS</span><span class="sxs-lookup"><span data-stu-id="03694-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="03694-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="03694-119">**Twilio:**</span></span>  
<span data-ttu-id="03694-120">Na kartě řídicí panel svého účtu Twilio zkopírovat **SID účtu** a **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="03694-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="03694-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="03694-121">**ASPSMS:**</span></span>  
<span data-ttu-id="03694-122">V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej společně s vaší **heslo**.</span><span class="sxs-lookup"><span data-stu-id="03694-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="03694-123">Později jsme uloží tyto hodnoty pomocí nástroje Správce tajný klíč v rámci klíče `SMSAccountIdentification` a `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="03694-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="03694-124">Zadání ID odesílatele nebo původce</span><span class="sxs-lookup"><span data-stu-id="03694-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="03694-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="03694-125">**Twilio:**</span></span>  
<span data-ttu-id="03694-126">Na kartě čísla zkopírujte vaše Twilio **telefonní číslo**.</span><span class="sxs-lookup"><span data-stu-id="03694-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="03694-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="03694-127">**ASPSMS:**</span></span>  
<span data-ttu-id="03694-128">V nabídce autoru odemknutí odemknutí jeden nebo více autoru nebo zvolte alfanumerické původce (nepodporuje všechny sítě).</span><span class="sxs-lookup"><span data-stu-id="03694-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="03694-129">Později jsme uloží tuto hodnotu pomocí nástroje Správce tajný klíč v rámci klíč `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="03694-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="03694-130">Zadejte přihlašovací údaje pro službu SMS</span><span class="sxs-lookup"><span data-stu-id="03694-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="03694-131">Použijeme [možnosti vzor](xref:fundamentals/configuration/options) pro přístup k účtu a klíč nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="03694-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="03694-132">Vytvořte třídu načíst zabezpečený klíč serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="03694-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="03694-133">Tato ukázka `SMSoptions` je v vytvořit třídu *Services/SMSoptions.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="03694-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="03694-134">Nastavte `SMSAccountIdentification`, `SMSAccountPassword` a `SMSAccountFrom` s [nástroj tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="03694-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="03694-135">Příklad:</span><span class="sxs-lookup"><span data-stu-id="03694-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="03694-136">Přidejte balíček NuGet pro poskytovatele služby SMS.</span><span class="sxs-lookup"><span data-stu-id="03694-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="03694-137">Z balíček správce konzoly (pomocí PMC) spusťte:</span><span class="sxs-lookup"><span data-stu-id="03694-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="03694-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="03694-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="03694-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="03694-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="03694-140">Přidejte kód v *Services/MessageServices.cs* souboru SMS.</span><span class="sxs-lookup"><span data-stu-id="03694-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="03694-141">Použijte Twilio nebo ASPSMS části:</span><span class="sxs-lookup"><span data-stu-id="03694-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="03694-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="03694-142">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="03694-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="03694-143">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="03694-144">Konfigurace spuštění používat`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="03694-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="03694-145">Přidat `SMSoptions` ke kontejneru služby v `ConfigureServices` metoda v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="03694-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="03694-146">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="03694-146">Enable two-factor authentication</span></span>

<span data-ttu-id="03694-147">Otevřete *Views/Manage/Index.cshtml* souboru nástroje Razor zobrazení a odebrání komentář znaků (takže žádný kód je odkomentovaný).</span><span class="sxs-lookup"><span data-stu-id="03694-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="03694-148">Přihlaste se pomocí dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="03694-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="03694-149">Spusťte aplikaci a zaregistrovat nového uživatele</span><span class="sxs-lookup"><span data-stu-id="03694-149">Run the app and register a new user</span></span>

![Webovou aplikaci zaregistrovat zobrazení otevřít v Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="03694-151">Klepněte na jméno uživatele, který aktivuje `Index` metodu akce v kontroleru spravovat.</span><span class="sxs-lookup"><span data-stu-id="03694-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="03694-152">Potom klepněte na telefonní číslo **přidat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="03694-152">Then tap the phone number **Add** link.</span></span>

![Správa zobrazení](2fa/_static/login2fa2.png)

* <span data-ttu-id="03694-154">Telefonní číslo, který bude přijímat ověřovací kód a klepněte na Přidat **Odeslat ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="03694-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Přidat stránku telefonní číslo](2fa/_static/login2fa3.png)

* <span data-ttu-id="03694-156">Zobrazí se zpráva SMS s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="03694-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="03694-157">Zadejte ji a klepněte na **odeslání**</span><span class="sxs-lookup"><span data-stu-id="03694-157">Enter it and tap **Submit**</span></span>

![Zkontrolujte telefonní číslo stránky](2fa/_static/login2fa4.png)

<span data-ttu-id="03694-159">Pokud neobdržíte textovou zprávu, najdete v protokolu stránky twilio.</span><span class="sxs-lookup"><span data-stu-id="03694-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="03694-160">Správa zobrazení ukazuje, že váš telefon byl úspěšně přidán.</span><span class="sxs-lookup"><span data-stu-id="03694-160">The Manage view shows your phone number was added successfully.</span></span>

![Správa zobrazení](2fa/_static/login2fa5.png)

* <span data-ttu-id="03694-162">Klepněte na **povolit** povolit dvoufaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="03694-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Správa zobrazení](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="03694-164">Test dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="03694-164">Test two-factor authentication</span></span>

* <span data-ttu-id="03694-165">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="03694-165">Log off.</span></span>

* <span data-ttu-id="03694-166">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="03694-166">Log in.</span></span>

* <span data-ttu-id="03694-167">Uživatelský účet má povoleno dvoufaktorové ověřování, je nutné zadat druhý faktor ověřování.</span><span class="sxs-lookup"><span data-stu-id="03694-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="03694-168">V tomto kurzu jste povolili ověření telefonu.</span><span class="sxs-lookup"><span data-stu-id="03694-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="03694-169">Integrované šablony lze také nastavit e-mail. jako druhý faktor.</span><span class="sxs-lookup"><span data-stu-id="03694-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="03694-170">Můžete nastavit další druhý faktory ověřování, jako je kódy QR.</span><span class="sxs-lookup"><span data-stu-id="03694-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="03694-171">Klepněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="03694-171">Tap **Submit**.</span></span>

![Odeslat ověřovací kód zobrazení](2fa/_static/login2fa7.png)

* <span data-ttu-id="03694-173">Zadejte kód, který se zobrazí ve zprávě SMS.</span><span class="sxs-lookup"><span data-stu-id="03694-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="03694-174">Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení při použití stejné zařízení a prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="03694-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="03694-175">Povolení 2FA a kliknete na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="03694-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="03694-176">Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="03694-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="03694-177">Nastavením **mějte na paměti, tento prohlížeč**, získáte další zabezpečení 2FA ze zařízení, nepoužívejte pravidelně a získat užitečný na nebudou muset projít 2FA na vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="03694-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Ověření zobrazení](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="03694-179">Uzamčení účtu pro ochranu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="03694-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="03694-180">Doporučujeme, že abyste použili uzamčení účtu s 2FA.</span><span class="sxs-lookup"><span data-stu-id="03694-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="03694-181">Jakmile se uživatel přihlásí (prostřednictvím místního účtu nebo sociální účtu), je uložen v 2FA každý neúspěšný pokus a pokud bude dosažen maximální počet pokusů o (výchozí hodnota je 5), uživatel je uzamčen pět minut (můžete nastavit uzamčení časem `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="03694-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="03694-182">Následující nakonfiguruje účet uzamčen po dobu 10 minut po 10 neúspěšných pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="03694-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
