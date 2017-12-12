---
title: "Dvoufaktorové ověřování pomocí serveru SMS"
author: rick-anderson
description: "Ukazuje, jak nastavit dvoufaktorové ověřování (2FA) s ASP.NET Core"
keywords: "ASP.NET Core, SMS, ověřování, 2FA, dvoufaktorové ověřování, dvoufaktorového ověřování"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 15620d89c4db2e74dbcec4707bb2ebc6df916e03
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="cfc02-104">Dvoufaktorové ověřování pomocí serveru SMS</span><span class="sxs-lookup"><span data-stu-id="cfc02-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="cfc02-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [mezi Devs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="cfc02-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="cfc02-106">V tomto kurzu platí pro ASP.NET Core pouze 1.x.</span><span class="sxs-lookup"><span data-stu-id="cfc02-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="cfc02-107">V tématu [vygenerovat kód QR povolení pro aplikace v ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pro technologii ASP.NET 2.0 jádra a novější.</span><span class="sxs-lookup"><span data-stu-id="cfc02-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="cfc02-108">Tento kurz ukazuje, jak nastavit dvoufaktorové ověřování (2FA) pomocí serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="cfc02-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="cfc02-109">Jsou uvedeny pokyny pro [twilio](https://www.twilio.com/) a [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale můžete použít další poskytovatele serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="cfc02-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="cfc02-110">Doporučujeme, abyste dokončení [potvrzení účtu a heslo pro obnovení](accconfirm.md) před zahájením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="cfc02-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="cfc02-111">Zobrazení [hotová ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="cfc02-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="cfc02-112">[Stažení](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="cfc02-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="cfc02-113">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfc02-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="cfc02-114">Vytvořit novou webovou aplikaci ASP.NET Core s názvem `Web2FA` s jednotlivých uživatelských účtů.</span><span class="sxs-lookup"><span data-stu-id="cfc02-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="cfc02-115">Postupujte podle pokynů v [vynucování SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) nastavit a vyžadovat protokol SSL.</span><span class="sxs-lookup"><span data-stu-id="cfc02-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="cfc02-116">Vytvoření účtu služby SMS</span><span class="sxs-lookup"><span data-stu-id="cfc02-116">Create an SMS account</span></span>

<span data-ttu-id="cfc02-117">Vytvoření účtu služby SMS, například z [twilio](https://www.twilio.com/) nebo [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="cfc02-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="cfc02-118">Zaznamenejte ověřovací pověření (pro twilio: accountSid a ověřovacího tokenu pro ASPSMS: Userkey a heslo).</span><span class="sxs-lookup"><span data-stu-id="cfc02-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="cfc02-119">Zjištění přihlašovací údaje poskytovatele služby SMS</span><span class="sxs-lookup"><span data-stu-id="cfc02-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="cfc02-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-120">**Twilio:**</span></span>  
<span data-ttu-id="cfc02-121">Na kartě řídicí panel svého účtu Twilio zkopírovat **SID účtu** a **Auth token**.</span><span class="sxs-lookup"><span data-stu-id="cfc02-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="cfc02-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-122">**ASPSMS:**</span></span>  
<span data-ttu-id="cfc02-123">V nastavení svého účtu, přejděte na **Userkey** a zkopírujte jej společně s vaší **heslo**.</span><span class="sxs-lookup"><span data-stu-id="cfc02-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="cfc02-124">Později jsme uloží tyto hodnoty pomocí nástroje Správce tajný klíč v rámci klíče `SMSAccountIdentification` a `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="cfc02-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="cfc02-125">Zadání ID odesílatele nebo původce</span><span class="sxs-lookup"><span data-stu-id="cfc02-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="cfc02-126">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-126">**Twilio:**</span></span>  
<span data-ttu-id="cfc02-127">Na kartě čísla zkopírujte vaše Twilio **telefonní číslo**.</span><span class="sxs-lookup"><span data-stu-id="cfc02-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="cfc02-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-128">**ASPSMS:**</span></span>  
<span data-ttu-id="cfc02-129">V nabídce autoru odemknutí odemknutí jeden nebo více autoru nebo zvolte alfanumerické původce (nepodporuje všechny sítě).</span><span class="sxs-lookup"><span data-stu-id="cfc02-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="cfc02-130">Později jsme uloží tuto hodnotu pomocí nástroje Správce tajný klíč v rámci klíč `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="cfc02-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="cfc02-131">Zadejte přihlašovací údaje pro službu SMS</span><span class="sxs-lookup"><span data-stu-id="cfc02-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="cfc02-132">Použijeme [možnosti vzor](xref:fundamentals/configuration/options) pro přístup k účtu a klíč nastavení uživatele.</span><span class="sxs-lookup"><span data-stu-id="cfc02-132">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="cfc02-133">Vytvořte třídu načíst zabezpečený klíč serveru SMS.</span><span class="sxs-lookup"><span data-stu-id="cfc02-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="cfc02-134">Tato ukázka `SMSoptions` je v vytvořit třídu *Services/SMSoptions.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="cfc02-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="cfc02-135">Nastavte `SMSAccountIdentification`, `SMSAccountPassword` a `SMSAccountFrom` s [nástroj tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="cfc02-135">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="cfc02-136">Příklad:</span><span class="sxs-lookup"><span data-stu-id="cfc02-136">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="cfc02-137">Přidejte balíček NuGet pro poskytovatele služby SMS.</span><span class="sxs-lookup"><span data-stu-id="cfc02-137">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="cfc02-138">Z balíček správce konzoly (pomocí PMC) spusťte:</span><span class="sxs-lookup"><span data-stu-id="cfc02-138">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="cfc02-139">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-139">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="cfc02-140">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-140">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="cfc02-141">Přidejte kód v *Services/MessageServices.cs* souboru SMS.</span><span class="sxs-lookup"><span data-stu-id="cfc02-141">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="cfc02-142">Použijte Twilio nebo ASPSMS části:</span><span class="sxs-lookup"><span data-stu-id="cfc02-142">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="cfc02-143">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-143">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="cfc02-144">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="cfc02-144">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="cfc02-145">Konfigurace spuštění používat`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="cfc02-145">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="cfc02-146">Přidat `SMSoptions` ke kontejneru služby v `ConfigureServices` metoda v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cfc02-146">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="cfc02-147">Povolit dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="cfc02-147">Enable two-factor authentication</span></span>

<span data-ttu-id="cfc02-148">Otevřete *Views/Manage/Index.cshtml* souboru nástroje Razor zobrazení a odebrání komentář znaků (takže žádný kód je odkomentovaný).</span><span class="sxs-lookup"><span data-stu-id="cfc02-148">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="cfc02-149">Přihlaste se pomocí dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="cfc02-149">Log in with two-factor authentication</span></span>

* <span data-ttu-id="cfc02-150">Spusťte aplikaci a zaregistrovat nového uživatele</span><span class="sxs-lookup"><span data-stu-id="cfc02-150">Run the app and register a new user</span></span>

![Webovou aplikaci zaregistrovat zobrazení otevřít v Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="cfc02-152">Klepněte na jméno uživatele, který aktivuje `Index` metodu akce v kontroleru spravovat.</span><span class="sxs-lookup"><span data-stu-id="cfc02-152">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="cfc02-153">Potom klepněte na telefonní číslo **přidat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="cfc02-153">Then tap the phone number **Add** link.</span></span>

![Správa zobrazení](2fa/_static/login2fa2.png)

* <span data-ttu-id="cfc02-155">Telefonní číslo, který bude přijímat ověřovací kód a klepněte na Přidat **Odeslat ověřovací kód**.</span><span class="sxs-lookup"><span data-stu-id="cfc02-155">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Přidat stránku telefonní číslo](2fa/_static/login2fa3.png)

* <span data-ttu-id="cfc02-157">Zobrazí se zpráva SMS s ověřovacím kódem.</span><span class="sxs-lookup"><span data-stu-id="cfc02-157">You will get a text message with the verification code.</span></span> <span data-ttu-id="cfc02-158">Zadejte ji a klepněte na **odeslání**</span><span class="sxs-lookup"><span data-stu-id="cfc02-158">Enter it and tap **Submit**</span></span>

![Zkontrolujte telefonní číslo stránky](2fa/_static/login2fa4.png)

<span data-ttu-id="cfc02-160">Pokud neobdržíte textovou zprávu, najdete v protokolu stránky twilio.</span><span class="sxs-lookup"><span data-stu-id="cfc02-160">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="cfc02-161">Správa zobrazení ukazuje, že váš telefon byl úspěšně přidán.</span><span class="sxs-lookup"><span data-stu-id="cfc02-161">The Manage view shows your phone number was added successfully.</span></span>

![Správa zobrazení](2fa/_static/login2fa5.png)

* <span data-ttu-id="cfc02-163">Klepněte na **povolit** povolit dvoufaktorové ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfc02-163">Tap **Enable** to enable two-factor authentication.</span></span>

![Správa zobrazení](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="cfc02-165">Test dvoufaktorové ověřování</span><span class="sxs-lookup"><span data-stu-id="cfc02-165">Test two-factor authentication</span></span>

* <span data-ttu-id="cfc02-166">Odhlaste se.</span><span class="sxs-lookup"><span data-stu-id="cfc02-166">Log off.</span></span>

* <span data-ttu-id="cfc02-167">Přihlásit se.</span><span class="sxs-lookup"><span data-stu-id="cfc02-167">Log in.</span></span>

* <span data-ttu-id="cfc02-168">Uživatelský účet má povoleno dvoufaktorové ověřování, je nutné zadat druhý faktor ověřování.</span><span class="sxs-lookup"><span data-stu-id="cfc02-168">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="cfc02-169">V tomto kurzu jste povolili ověření telefonu.</span><span class="sxs-lookup"><span data-stu-id="cfc02-169">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="cfc02-170">Integrované šablony lze také nastavit e-mail. jako druhý faktor.</span><span class="sxs-lookup"><span data-stu-id="cfc02-170">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="cfc02-171">Můžete nastavit další druhý faktory ověřování, jako je kódy QR.</span><span class="sxs-lookup"><span data-stu-id="cfc02-171">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="cfc02-172">Klepněte na **odeslání**.</span><span class="sxs-lookup"><span data-stu-id="cfc02-172">Tap **Submit**.</span></span>

![Odeslat ověřovací kód zobrazení](2fa/_static/login2fa7.png)

* <span data-ttu-id="cfc02-174">Zadejte kód, který se zobrazí ve zprávě SMS.</span><span class="sxs-lookup"><span data-stu-id="cfc02-174">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="cfc02-175">Kliknutím na **mějte na paměti, tento prohlížeč** zaškrtávací políčko je vyloučit z nutnosti použít 2FA přihlášení při použití stejné zařízení a prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="cfc02-175">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="cfc02-176">Povolení 2FA a kliknete na **mějte na paměti, tento prohlížeč** vám poskytne silné 2FA ochrany z uživatelé se zlými úmysly pokusu o přístup k účtu, tak dlouho, dokud nemají přístup k zařízení.</span><span class="sxs-lookup"><span data-stu-id="cfc02-176">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="cfc02-177">Můžete provést na jakémkoli privátní zařízení, kterou používáte pravidelně.</span><span class="sxs-lookup"><span data-stu-id="cfc02-177">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="cfc02-178">Nastavením **mějte na paměti, tento prohlížeč**, získáte další zabezpečení 2FA ze zařízení, nepoužívejte pravidelně a získat užitečný na nebudou muset projít 2FA na vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="cfc02-178">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Ověření zobrazení](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="cfc02-180">Uzamčení účtu pro ochranu před útoky hrubou silou</span><span class="sxs-lookup"><span data-stu-id="cfc02-180">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="cfc02-181">Doporučujeme, že abyste použili uzamčení účtu s 2FA.</span><span class="sxs-lookup"><span data-stu-id="cfc02-181">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="cfc02-182">Jakmile se uživatel přihlásí (prostřednictvím místního účtu nebo sociální účtu), je uložen v 2FA každý neúspěšný pokus a pokud bude dosažen maximální počet pokusů o (výchozí hodnota je 5), uživatel je uzamčen pět minut (můžete nastavit uzamčení časem `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="cfc02-182">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="cfc02-183">Následující nakonfiguruje účet uzamčen po dobu 10 minut po 10 neúspěšných pokusů o přihlášení.</span><span class="sxs-lookup"><span data-stu-id="cfc02-183">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
