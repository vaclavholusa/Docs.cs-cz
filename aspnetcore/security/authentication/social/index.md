---
title: Facebook, Google a externí zprostředkovatel ověřování v ASP.NET Core
author: rick-anderson
description: Tento kurz ukazuje vytvoření ASP.NET Core 2.x aplikace pomocí externího zprostředkovatele ověřování OAuth 2.0.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: 48a01ab241f9a6ad6ad3fb2ee9e210f459075c33
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336117"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="6e1d9-103">Facebook, Google a externí zprostředkovatel ověřování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e1d9-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="6e1d9-104">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6e1d9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6e1d9-105">Tento kurz ukazuje, jak vytvářet v ASP.NET Core 2.x aplikaci, která umožňuje uživatelům přihlášení pomocí přihlašovacích údajů z externího zprostředkovatele ověřování OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="6e1d9-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), a [Microsoft](xref:security/authentication/microsoft-logins) poskytovatelé jsou popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="6e1d9-107">Ostatní zprostředkovatelé jsou k dispozici v balíčky třetích stran, jako [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) a [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="6e1d9-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony sociálních médií pro Facebook, Twitter, Google, plus a Windows](index/_static/social.png)

<span data-ttu-id="6e1d9-109">Umožňuje uživatelům přihlašovat se pomocí existujících přihlašovacích údajů je pro uživatele pohodlný a posune mnoho složitých úkolů při správě procesu přihlašování do jiného výrobce.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="6e1d9-110">Příklady, jak sociální přihlášení můžete jednotka provoz a zákazníka převody, naleznete v tématu případové studie podle [Facebook](https://www.facebook.com/unsupportedbrowser) a [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="6e1d9-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="6e1d9-111">Poznámka: Balíčky okomentovat abstraktní spoustu složitost tok ověřování OAuth, ale Princip podrobnosti může být nezbytné při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="6e1d9-112">Mnoho prostředků jsou k dispozici. Viz například [Úvod do OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) nebo [Principy OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="6e1d9-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="6e1d9-113">Některé problémy dají odstranit pohledem [ASP.NET Core zdrojový kód pro balíčky poskytovatele](https://github.com/aspnet/Security/tree/master/src).</span><span class="sxs-lookup"><span data-stu-id="6e1d9-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/master/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="6e1d9-114">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6e1d9-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="6e1d9-115">V sadě Visual Studio 2017, vytvořte nový projekt z úvodní stránky, nebo prostřednictvím **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="6e1d9-116">Vyberte **webové aplikace ASP.NET Core** šablony, které jsou k dispozici v **Visual C# > .NET Core** kategorie:</span><span class="sxs-lookup"><span data-stu-id="6e1d9-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Dialogové okno nového projektu](index/_static/new-project.png)

* <span data-ttu-id="6e1d9-118">Klepněte na **webovou aplikaci** a ověřte **ověřování** je nastavena na **jednotlivé uživatelské účty**:</span><span class="sxs-lookup"><span data-stu-id="6e1d9-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Dialogové okno nové webové aplikace](index/_static/select-project.png)

<span data-ttu-id="6e1d9-120">Poznámka: Tento kurz se vztahuje na verzi ASP.NET Core 2.0 SDK, která se dají v horní části průvodce.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="6e1d9-121">Použití migrace</span><span class="sxs-lookup"><span data-stu-id="6e1d9-121">Apply migrations</span></span>

* <span data-ttu-id="6e1d9-122">Spusťte aplikaci a vyberte **přihlášení** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="6e1d9-123">Vyberte **zaregistrujte se jako nový uživatel** odkaz.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="6e1d9-124">Zadejte e-mail a heslo pro nový účet a potom vyberte **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="6e1d9-125">Postupujte podle pokynů a použití migrace.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="6e1d9-126">Požadovat protokol SSL</span><span class="sxs-lookup"><span data-stu-id="6e1d9-126">Require SSL</span></span>

<span data-ttu-id="6e1d9-127">OAuth 2.0 vyžaduje použití protokolu SSL pro ověřování prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="6e1d9-128">Poznámka: Projekty vytvořené využitím **webovou aplikaci** nebo **webového rozhraní API** šablony projektů pro ASP.NET Core 2.x se automaticky konfigurují pro povolení protokolu SSL a spustit s adresou URL protokolu https, pokud **jednotlivé Uživatelské účty** na jste vybrali možnost **dialogové okno Změnit ověřování** v Průvodci vytvořením projektu, jak je znázorněno výše.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="6e1d9-129">Vyžadování protokolu SSL na vašem webu pomocí následujících kroků v [vynucování SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) tématu.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-129">Require SSL on your site by following the steps in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="6e1d9-130">Pomocí SecretManager ukládat tokeny přiřadil zprostředkovatele přihlášení</span><span class="sxs-lookup"><span data-stu-id="6e1d9-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="6e1d9-131">Přiřadit zprostředkovatele přihlášení prostřednictvím sociální sítě **Id aplikace** a **tajný klíč aplikace** tokeny během procesu registrace.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="6e1d9-132">Přesné token názvy lišit podle zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-132">The exact token names vary by provider.</span></span> <span data-ttu-id="6e1d9-133">Tyto tokeny představují přihlašovací údaje, které vaše aplikace používá pro přístup k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-133">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="6e1d9-134">Tokeny představují "tajné", které lze propojit s díky konfiguraci aplikací [manažera tajných](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="6e1d9-134">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="6e1d9-135">Tajný klíč správce je bezpečnější alternativu pro ukládání tokenů v konfiguračním souboru, například *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-135">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6e1d9-136">Tajný klíč správce je jenom pro účely vývoje.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-136">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="6e1d9-137">Můžete ukládat a chránit Azure testovací a produkční tajných kódů pomocí [poskytovatele konfigurace služby Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="6e1d9-137">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="6e1d9-138">Postupujte podle kroků v [bezpečné ukládání tajných kódů aplikace při vývoji v ASP.NET Core](xref:security/app-secrets) tématu pro ukládání tokenů přiřadil každý zprostředkovatele přihlášení níže.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-138">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="6e1d9-139">Instalační program zprostředkovatele přihlášení požadovaná vaší aplikací</span><span class="sxs-lookup"><span data-stu-id="6e1d9-139">Setup login providers required by your application</span></span>

<span data-ttu-id="6e1d9-140">Ke konfiguraci vaší aplikaci použít příslušné poskytovatele použijte následující témata:</span><span class="sxs-lookup"><span data-stu-id="6e1d9-140">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="6e1d9-141">[Facebook](xref:security/authentication/facebook-logins) pokyny</span><span class="sxs-lookup"><span data-stu-id="6e1d9-141">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="6e1d9-142">[Twitter](xref:security/authentication/twitter-logins) pokyny</span><span class="sxs-lookup"><span data-stu-id="6e1d9-142">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="6e1d9-143">[Google](xref:security/authentication/google-logins) pokyny</span><span class="sxs-lookup"><span data-stu-id="6e1d9-143">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="6e1d9-144">[Microsoft](xref:security/authentication/microsoft-logins) pokyny</span><span class="sxs-lookup"><span data-stu-id="6e1d9-144">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="6e1d9-145">[Jiný poskytovatel](xref:security/authentication/otherlogins) pokyny</span><span class="sxs-lookup"><span data-stu-id="6e1d9-145">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="6e1d9-146">Volitelně můžete nastavit heslo</span><span class="sxs-lookup"><span data-stu-id="6e1d9-146">Optionally set password</span></span>

<span data-ttu-id="6e1d9-147">Při registraci se externího zprostředkovatele přihlášení, není nutné heslo registrován s aplikací.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-147">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="6e1d9-148">To řeší jste od vytvoření a zapamatování hesla pro lokalitu, ale také umožňuje závisí na externího zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-148">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="6e1d9-149">Pokud není k dispozici na externího zprostředkovatele přihlášení, nebudete moct přihlásit k webu.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-149">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="6e1d9-150">Chcete-li vytvořit heslo a přihlaste se pomocí e-mailu, kterou jste nastavili během procesu přihlašování s externí zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="6e1d9-150">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="6e1d9-151">Klepněte **Hello &lt;e-mailový alias&gt;**  v pravém horním rohu přejít na odkaz **spravovat** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-151">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Zobrazení Správa webové aplikace](index/_static/pass1a.png)

* <span data-ttu-id="6e1d9-153">Klepněte na **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="6e1d9-153">Tap **Create**</span></span>

![Nastavte heslo stránku](index/_static/pass2a.png)

* <span data-ttu-id="6e1d9-155">Nastavili platné heslo a může být využit k přihlášení pomocí e-mailu.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-155">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e1d9-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e1d9-156">Next steps</span></span>

* <span data-ttu-id="6e1d9-157">Tento článek zavedené externího ověřování a vysvětlení požadovaných součástí pro přidání externí přihlášení do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-157">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="6e1d9-158">Referenční stránky specifickým pro zprostředkovatele konfigurace přihlášení pro zprostředkovatele, které vaše aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-158">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="6e1d9-159">Můžete chtít zachovat další data o uživateli a jejich přístup a obnovovací tokeny.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-159">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="6e1d9-160">Další informace naleznete v tématu <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="6e1d9-160">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
