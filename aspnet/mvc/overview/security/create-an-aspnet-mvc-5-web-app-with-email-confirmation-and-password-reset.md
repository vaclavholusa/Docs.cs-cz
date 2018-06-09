---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Vytvoření zabezpečeného webové aplikace ASP.NET MVC 5 se přihlásit, e-mailem potvrzení a heslo resetovat (C#) | Microsoft Docs
author: Rick-Anderson
description: Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s potvrzení e-mailu a resetování hesla pomocí systému členství ASP.NET Identity. Můžete certifikační autority...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "34452565"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="f1d12-104">Vytvoření zabezpečeného webové aplikace ASP.NET MVC 5 se přihlásit, e-mailem potvrzení a heslo resetovat (C#)</span><span class="sxs-lookup"><span data-stu-id="f1d12-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="f1d12-105">podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f1d12-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="f1d12-106">Tento kurz ukazuje, jak sestavit webové aplikace ASP.NET MVC 5 s potvrzení e-mailu a resetování hesla pomocí systému členství ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="f1d12-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="f1d12-107">Hotová aplikace si můžete stáhnout [zde](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="f1d12-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="f1d12-108">Stahování obsahuje ladění pomocné rutiny, které umožňují testování potvrzení e-mailu a SMS bez nastavení e-mailem nebo poskytovatele služby SMS.</span><span class="sxs-lookup"><span data-stu-id="f1d12-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="f1d12-109">V tomto kurzu napsal [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="f1d12-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="f1d12-110">Vytvoření aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f1d12-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="f1d12-111">Začněte tím, že instalace a spuštění [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="f1d12-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="f1d12-112">Nainstalujte [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="f1d12-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="f1d12-113">Upozornění: Je nutné nainstalovat [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) nebo vyšší k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="f1d12-114">Vytvořte nový projekt ASP.NET Web a vyberte šablonu MVC.</span><span class="sxs-lookup"><span data-stu-id="f1d12-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="f1d12-115">Webové formuláře také podporuje ASP.NET Identity, takže můžete sledovat v aplikaci web forms podobným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f1d12-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="f1d12-116">Ponechte výchozí ověřování jako **jednotlivé uživatelské účty**.</span><span class="sxs-lookup"><span data-stu-id="f1d12-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="f1d12-117">Pokud chcete hostovat aplikaci v Azure, nechte políčko zaškrtnuto.</span><span class="sxs-lookup"><span data-stu-id="f1d12-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="f1d12-118">Později v tomto kurzu jsme nasadit do Azure.</span><span class="sxs-lookup"><span data-stu-id="f1d12-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="f1d12-119">Můžete [zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="f1d12-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="f1d12-120">Nastavte [projektu pro použití protokolu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="f1d12-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="f1d12-121">Spuštění aplikace, klikněte na tlačítko **zaregistrovat** propojit a zaregistrovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="f1d12-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="f1d12-122">V tomto okamžiku je pouze ověření na e-mailu [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atribut.</span><span class="sxs-lookup"><span data-stu-id="f1d12-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="f1d12-123">V Průzkumníku serveru, přejděte na **Data Connections\DefaultConnection\Tables\AspNetUsers**, klikněte pravým tlačítkem a vyberte **otevřete definici tabulky**.</span><span class="sxs-lookup"><span data-stu-id="f1d12-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="f1d12-124">Na následujícím obrázku `AspNetUsers` schématu:</span><span class="sxs-lookup"><span data-stu-id="f1d12-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="f1d12-125">Klikněte pravým tlačítkem na **AspNetUsers** tabulky a vyberte **zobrazit Data tabulky**.</span><span class="sxs-lookup"><span data-stu-id="f1d12-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="f1d12-126">V tomto okamžiku není potvrzena e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="f1d12-127">Klikněte na řádek, a vyberte možnost odstranit.</span><span class="sxs-lookup"><span data-stu-id="f1d12-127">Click on the row and select delete.</span></span> <span data-ttu-id="f1d12-128">Budete v dalším kroku znovu přidat tuto e-mailu a odešlete e-mail s potvrzením.</span><span class="sxs-lookup"><span data-stu-id="f1d12-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="f1d12-129">Potvrzení e-mailu</span><span class="sxs-lookup"><span data-stu-id="f1d12-129">Email confirmation</span></span>

<span data-ttu-id="f1d12-130">Je osvědčeným postupem potvrďte e-mailu nové registrace uživatele ověřit, že nejsou zosobnění někdo jiný (to znamená, že nebyly zaregistrována někoho jiného e-mailu).</span><span class="sxs-lookup"><span data-stu-id="f1d12-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="f1d12-131">Předpokládejme, že jste měli diskusní fórum, chcete zabránit `"bob@example.com"` registraci jako `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="f1d12-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="f1d12-132">Bez potvrzení e-mailu `"joe@contoso.com"` může získat nežádoucí e-mailu z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1d12-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="f1d12-133">Předpokládejme, že Bob, ať už náhodně registrován jako `"bib@example.com"` a kdyby si všimli, si nebude moct používat heslo obnovit, protože aplikace nemá správnou e-mailovou.</span><span class="sxs-lookup"><span data-stu-id="f1d12-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="f1d12-134">Potvrzení e-mailu poskytuje jen omezenou ochranu z robotů a neposkytuje ochranu proti určené odesílatelům nevyžádané pošty, mají mnoho aliasy pracovní e-mailu, které můžete použít k registraci.</span><span class="sxs-lookup"><span data-stu-id="f1d12-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="f1d12-135">Obvykle budete chtít zabránit noví uživatelé předtím, než byl potvrzen e-mailu, textovou zprávu SMS nebo jiný mechanismus publikování všechna data na webové stránky.</span><span class="sxs-lookup"><span data-stu-id="f1d12-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="f1d12-136">V následujících částech se budeme povolí potvrzení e-mailu a upravit kód nově zaregistrovaný uživatelům zabránit protokolování, dokud bylo potvrzeno e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="f1d12-137">Propojte sendgrid vám umožňuje</span><span class="sxs-lookup"><span data-stu-id="f1d12-137">Hook up SendGrid</span></span>

<span data-ttu-id="f1d12-138">I když tento kurz ukazuje jenom postup přidání e-mailové oznámení prostřednictvím [sendgrid vám umožňuje](http://sendgrid.com/), můžete odeslat e-mailu pomocí protokolu SMTP a další mechanismy (viz [další prostředky](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="f1d12-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="f1d12-139">V konzole Správce balíčků zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f1d12-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="f1d12-140">Přejděte na [sendgrid vám umožňuje Azure registrační stránku](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) a zaregistrovat bezplatný účet sendgrid vám umožňuje.</span><span class="sxs-lookup"><span data-stu-id="f1d12-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="f1d12-141">Sendgrid vám umožňuje nakonfigurovat tak, že přidáte kód, podobně jako v následujícím *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1d12-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="f1d12-142">Budete muset přidat že následující obsahuje:</span><span class="sxs-lookup"><span data-stu-id="f1d12-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="f1d12-143">Pro zjednodušení této ukázku, uložíme nastavení aplikace v *web.config* souboru:</span><span class="sxs-lookup"><span data-stu-id="f1d12-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="f1d12-144">Zabezpečení – nikdy úložiště citlivá data ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="f1d12-145">Účet a přihlašovací údaje jsou uloženy v appSetting.</span><span class="sxs-lookup"><span data-stu-id="f1d12-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="f1d12-146">V Azure, můžete bezpečně uložit tyto hodnoty na **[konfigurace](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="f1d12-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="f1d12-147">V tématu [osvědčené postupy pro nasazování hesel a dalších citlivých dat do ASP.NET a do Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="f1d12-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="f1d12-148">Povolit potvrzení e-mailu v kontroleru účtu</span><span class="sxs-lookup"><span data-stu-id="f1d12-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="f1d12-149">Ověřte *Views\Account\ConfirmEmail.cshtml* soubor má syntaxi razor správné.</span><span class="sxs-lookup"><span data-stu-id="f1d12-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="f1d12-150">(@ – Znak v prvním řádku chybět.</span><span class="sxs-lookup"><span data-stu-id="f1d12-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="f1d12-151">)</span><span class="sxs-lookup"><span data-stu-id="f1d12-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="f1d12-152">Spusťte aplikaci a klikněte na odkaz registrace.</span><span class="sxs-lookup"><span data-stu-id="f1d12-152">Run the app and click the Register link.</span></span> <span data-ttu-id="f1d12-153">Po odeslání formuláře registrace jste přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f1d12-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="f1d12-154">Zkontrolujte e-mailový účet a klikněte na odkaz k potvrzení e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="f1d12-155">Požadovat potvrzení e-mailu před přihlášení</span><span class="sxs-lookup"><span data-stu-id="f1d12-155">Require email confirmation before log in</span></span>

<span data-ttu-id="f1d12-156">Aktuálně po dokončení registračním formuláři Uživatel se přihlásí.</span><span class="sxs-lookup"><span data-stu-id="f1d12-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="f1d12-157">Chcete obecně potvrzení e-mailu před jejich protokolování.</span><span class="sxs-lookup"><span data-stu-id="f1d12-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="f1d12-158">V následující části jsme se změnit kód tak, aby vyžadovala nové uživatele tak, aby měl potvrzen e-mailu, než se přihlásí (ověřený).</span><span class="sxs-lookup"><span data-stu-id="f1d12-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="f1d12-159">Aktualizace `HttpPost Register` metoda s následující zvýrazněný změny:</span><span class="sxs-lookup"><span data-stu-id="f1d12-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="f1d12-160">Pomocí komentářů se `SignInAsync` metoda, nebude uživatel přihlášený pomocí registrace.</span><span class="sxs-lookup"><span data-stu-id="f1d12-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="f1d12-161">`TempData["ViewBagLink"] = callbackUrl;` Řádku lze provádět [ladění aplikace](#dbg) a testování registrace bez odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="f1d12-162">`ViewBag.Message` slouží k zobrazení potvrdit pokynů.</span><span class="sxs-lookup"><span data-stu-id="f1d12-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="f1d12-163">[Stažení ukázky](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) obsahuje kód pro testování potvrzení e-mailu bez nastavení e-mailu a můžete také použít k ladění aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1d12-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="f1d12-164">Vytvoření `Views\Shared\Info.cshtml` souboru a přidejte následující kód razor:</span><span class="sxs-lookup"><span data-stu-id="f1d12-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="f1d12-165">Přidat [autorizovat atribut](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) k `Contact` metody akce kontroleru domovské.</span><span class="sxs-lookup"><span data-stu-id="f1d12-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="f1d12-166">Kliknutím na **kontaktujte** odkaz ověřte anonymní uživatelé nemají přístup k a ověření uživatelé mají přístup.</span><span class="sxs-lookup"><span data-stu-id="f1d12-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="f1d12-167">Také musíte aktualizovat `HttpPost Login` metodu akce:</span><span class="sxs-lookup"><span data-stu-id="f1d12-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="f1d12-168">Aktualizace *Views\Shared\Error.cshtml* zobrazení chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="f1d12-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="f1d12-169">Odstranit všechny účty v **AspNetUsers** tabulku, která obsahují e-mailový alias, který chcete otestovat s.</span><span class="sxs-lookup"><span data-stu-id="f1d12-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="f1d12-170">Spusťte aplikaci a ověřte, že se nemůže přihlásit až se ujistíte, e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="f1d12-171">Jakmile potvrdíte, e-mailovou adresu, klikněte na **obraťte se na** odkaz.</span><span class="sxs-lookup"><span data-stu-id="f1d12-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="f1d12-172">Obnovení nebo resetování hesla</span><span class="sxs-lookup"><span data-stu-id="f1d12-172">Password recovery/reset</span></span>

<span data-ttu-id="f1d12-173">Odeberte komentář znaky z `HttpPost ForgotPassword` metody akce v kontroleru účet:</span><span class="sxs-lookup"><span data-stu-id="f1d12-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="f1d12-174">Odeberte komentář znaky z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) v *Views\Account\Login.cshtml* souboru nástroje razor zobrazení:</span><span class="sxs-lookup"><span data-stu-id="f1d12-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="f1d12-175">Přihlašovací stránky se nyní zobrazí odkaz k resetování hesla.</span><span class="sxs-lookup"><span data-stu-id="f1d12-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="f1d12-176">Odkaz pro potvrzení znovu odeslat e-mailu</span><span class="sxs-lookup"><span data-stu-id="f1d12-176">Resend email confirmation link</span></span>

<span data-ttu-id="f1d12-177">Jakmile uživatel vytvoří nový místní účet, jsou e-mailem potvrzení propojení, které musí předtím, než se nemohou přihlásit</span><span class="sxs-lookup"><span data-stu-id="f1d12-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="f1d12-178">Pokud uživatel, ať už náhodně odstraní e-mail s potvrzením nebo e-mailu nikdy dorazí, potřebují odkaz potvrzení odeslat znovu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="f1d12-179">Následující změny kódu ukazují, jak povolit tuto funkci.</span><span class="sxs-lookup"><span data-stu-id="f1d12-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="f1d12-180">Přidejte následující metodu helper k dolnímu okraji *Controllers\AccountController.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="f1d12-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="f1d12-181">Aktualizujte metodu registrace k použití nového pomocníka:</span><span class="sxs-lookup"><span data-stu-id="f1d12-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="f1d12-182">Aktualizujte metodu přihlášení o odeslání hesla, pokud nebyla potvrzena uživatelský účet:</span><span class="sxs-lookup"><span data-stu-id="f1d12-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="f1d12-183">Kombinování sociálních a místní přihlášení účtů</span><span class="sxs-lookup"><span data-stu-id="f1d12-183">Combine social and local login accounts</span></span>

<span data-ttu-id="f1d12-184">Kliknutím na odkaz na vaši e-mailu můžete kombinovat místní a sociálních účty.</span><span class="sxs-lookup"><span data-stu-id="f1d12-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="f1d12-185">V tomto pořadí **RickAndMSFT@gmail.com** je poprvé vytvořen jako místní přihlášení, ale můžete vytvořit účet jako sociálních protokolu v prvním a pak přidejte místní přihlášení.</span><span class="sxs-lookup"><span data-stu-id="f1d12-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="f1d12-186">Klikněte na **spravovat** odkaz.</span><span class="sxs-lookup"><span data-stu-id="f1d12-186">Click on the **Manage** link.</span></span> <span data-ttu-id="f1d12-187">Poznámka: **externích přihlášení: 0** spojené s tímto účtem.</span><span class="sxs-lookup"><span data-stu-id="f1d12-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="f1d12-188">Klikněte na odkaz na jiný protokol ve službě service a přijímat žádosti o aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1d12-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="f1d12-189">Dva účty jsou spojena, nebudete moci přihlásit pomocí buď účtu.</span><span class="sxs-lookup"><span data-stu-id="f1d12-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="f1d12-190">Můžete chtít uživatelům v případě, že jejich sociální přihlášení ověřovací služby, jsou vypnuty nebo jejich více pravděpodobně ztratili přístup k účtu mají sociálních přidat místní účty.</span><span class="sxs-lookup"><span data-stu-id="f1d12-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="f1d12-191">Na následujícím obrázku je tní sociálních přihlášení (což je vidět na **externích přihlášení: 1** zobrazený na stránce).</span><span class="sxs-lookup"><span data-stu-id="f1d12-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="f1d12-192">Kliknutím na **vyberte heslo** umožňuje přidat místního protokolu na související se stejným účtem.</span><span class="sxs-lookup"><span data-stu-id="f1d12-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="f1d12-193">Potvrzení e-mailu do větší hloubky</span><span class="sxs-lookup"><span data-stu-id="f1d12-193">Email confirmation in more depth</span></span>

<span data-ttu-id="f1d12-194">Moje kurzu [potvrzení účtu a heslo pro obnovení s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do tohoto tématu s dalšími podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="f1d12-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="f1d12-195">Ladění aplikace</span><span class="sxs-lookup"><span data-stu-id="f1d12-195">Debugging the app</span></span>

<span data-ttu-id="f1d12-196">Pokud neobdržíte e-mail, který obsahuje odkaz:</span><span class="sxs-lookup"><span data-stu-id="f1d12-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="f1d12-197">Zkontrolujte složku Nevyžádaná pošta nebo nevyžádané pošty.</span><span class="sxs-lookup"><span data-stu-id="f1d12-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="f1d12-198">Přihlaste se k účtu sendgrid vám umožňuje a klikněte na [odkaz e-mailu aktivity](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="f1d12-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="f1d12-199">Chcete-li otestovat ověřovací odkaz bez e-mailu, stáhněte [hotová ukázka](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="f1d12-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="f1d12-200">Odkaz pro potvrzení a kódy potvrzení zobrazí na stránce.</span><span class="sxs-lookup"><span data-stu-id="f1d12-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="f1d12-201">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="f1d12-201">Additional Resources</span></span>

- [<span data-ttu-id="f1d12-202">Odkazy na identitě ASP.NET Identity doporučené prostředky</span><span class="sxs-lookup"><span data-stu-id="f1d12-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="f1d12-203">[Účet potvrzení a obnovení hesla s ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) přejde do další podrobnosti o potvrzení hesla obnovení a účet.</span><span class="sxs-lookup"><span data-stu-id="f1d12-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="f1d12-204">[Aplikace MVC 5 se službou Facebook, Twitter, LinkedIn a přihlašování Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) tohoto kurzu se dozvíte, jak psát aplikace ASP.NET MVC 5 s ověřování sítě Facebook a Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="f1d12-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="f1d12-205">Také ukazuje, jak přidat další data do databáze Identity.</span><span class="sxs-lookup"><span data-stu-id="f1d12-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="f1d12-206">[Nasazení aplikace zabezpečené rozhraní ASP.NET MVC s členství, OAuth a databáze SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="f1d12-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="f1d12-207">V tomto kurzu přidá nasazení Azure, jak zabezpečit vaše aplikace s rolemi, jak používat členské rozhraní API k přidání uživatelů a rolí a další funkce zabezpečení.</span><span class="sxs-lookup"><span data-stu-id="f1d12-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="f1d12-208">Vytvoření aplikace na Google OAuth 2 a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="f1d12-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="f1d12-209">Vytvoření aplikace v síti Facebook a připojení aplikace k projektu</span><span class="sxs-lookup"><span data-stu-id="f1d12-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="f1d12-210">Nastavení protokolu SSL v projektu</span><span class="sxs-lookup"><span data-stu-id="f1d12-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
