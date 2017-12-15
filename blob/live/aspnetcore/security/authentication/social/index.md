---
title: "Povolení ověřování pomocí Facebook, Google a dalších externích zprostředkovatelů"
author: rick-anderson
description: "Tento kurz ukazuje, jak sestavit ASP.NET Core 2.x aplikaci pomocí externí zprostředkovatelé ověřování OAuth 2.0."
keywords: "ASP.NET Core, ověřování, sociální, zprostředkovatele ověřování, google, facebook, twitter, účet microsoft"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 9cc637f469dcb7097ee1b3996fde8a4ebac8d7ff
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a><span data-ttu-id="3466b-104">Povolení ověřování pomocí Facebook, Google a dalších externích zprostředkovatelů</span><span class="sxs-lookup"><span data-stu-id="3466b-104">Enabling authentication using Facebook, Google, and other external providers</span></span>

<a name="security-authentication-social-logins"></a>

<span data-ttu-id="3466b-105">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3466b-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3466b-106">Tento kurz ukazuje, jak sestavit ASP.NET Core 2.x aplikaci, která umožňuje uživatelům přihlásit se pomocí pověření z externí zprostředkovatelé ověřování OAuth 2.0.</span><span class="sxs-lookup"><span data-stu-id="3466b-106">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="3466b-107">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), a [Microsoft](microsoft-logins.md) poskytovatelé jsou popsané v následujících částech.</span><span class="sxs-lookup"><span data-stu-id="3466b-107">[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), and [Microsoft](microsoft-logins.md) providers are covered in the following sections.</span></span> <span data-ttu-id="3466b-108">Jiní poskytovatelé jsou k dispozici v balíčky jiných výrobců, jako [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) a [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="3466b-108">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Ikony sociálních médií pro Facebook, Twitter, Google plus a Windows](index/_static/social.png)

<span data-ttu-id="3466b-110">Povolení uživatelům přihlásit se pomocí přihlašovacích údajů existující je vhodné pro uživatele a posune řadu složitosti správy proces přihlášení do jiného výrobce.</span><span class="sxs-lookup"><span data-stu-id="3466b-110">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="3466b-111">Příklady, jak sociálních přihlášení můžete jednotka provozu a k zákaznickým převody, najdete v části případové studie podle [Facebook](https://www.facebook.com/unsupportedbrowser) a [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="3466b-111">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="3466b-112">Poznámka: Balíčky uvedené v této abstraktní značnou část složitosti tok ověřování OAuth, ale pochopení podrobnosti může být nutný při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="3466b-112">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="3466b-113">Mnoho prostředky jsou k dispozici. například v tématu [Úvod do OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) nebo [Principy OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="3466b-113">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="3466b-114">Některé problémy lze vyřešit prohlížením [ASP.NET Core zdrojového kódu pro balíčky zprostředkovatele](https://github.com/aspnet/Security/tree/dev/src).</span><span class="sxs-lookup"><span data-stu-id="3466b-114">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3466b-115">Vytvořte nový projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3466b-115">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="3466b-116">V aplikaci Visual Studio 2017, vytvořte nový projekt na stránce Start nebo prostřednictvím **soubor > Nový > projekt**.</span><span class="sxs-lookup"><span data-stu-id="3466b-116">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="3466b-117">Vyberte **webové aplikace ASP.NET Core** šablony, které jsou k dispozici v **Visual C# > .NET Core** kategorie:</span><span class="sxs-lookup"><span data-stu-id="3466b-117">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Dialogové okno Nový projekt](index/_static/new-project.png)

* <span data-ttu-id="3466b-119">Klepněte na **webové aplikace** a ověřte **ověřování** je nastaven na **jednotlivé uživatelské účty**:</span><span class="sxs-lookup"><span data-stu-id="3466b-119">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Dialogové okno nové webové aplikace](index/_static/select-project.png)

<span data-ttu-id="3466b-121">Poznámka: V tomto kurzu platí pro verze sady SDK technologie ASP.NET Core 2.0, které lze vybrat v horní části průvodce.</span><span class="sxs-lookup"><span data-stu-id="3466b-121">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="3466b-122">Použijte migrace</span><span class="sxs-lookup"><span data-stu-id="3466b-122">Apply migrations</span></span>

* <span data-ttu-id="3466b-123">Spusťte aplikaci a vyberte **přihlásit** odkaz.</span><span class="sxs-lookup"><span data-stu-id="3466b-123">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="3466b-124">Vyberte **zaregistrujte se jako nový uživatel** odkaz.</span><span class="sxs-lookup"><span data-stu-id="3466b-124">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="3466b-125">Zadejte e-mailu a heslo pro nový účet a potom vyberte **zaregistrovat**.</span><span class="sxs-lookup"><span data-stu-id="3466b-125">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="3466b-126">Postupujte podle pokynů pro použití migrace.</span><span class="sxs-lookup"><span data-stu-id="3466b-126">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="3466b-127">Požadovat protokol SSL</span><span class="sxs-lookup"><span data-stu-id="3466b-127">Require SSL</span></span>

<span data-ttu-id="3466b-128">OAuth 2.0 vyžaduje použití protokolu SSL pro ověřování prostřednictvím protokolu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3466b-128">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="3466b-129">Poznámka: Projekty vytvořené pomocí **webové aplikace** nebo **webového rozhraní API** šablony projektů pro ASP.NET Core 2.x automaticky konfigurují pro povolit protokol SSL a spustit s adresou URL https, pokud **jednotlivých Uživatelské účty** na jste vybrali možnost **dialogové okno Změna ověřování** v Průvodci projektu jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="3466b-129">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="3466b-130">Vyžadovat šifrování SSL na svém webu pomocí následujících kroků v [vynucování SSL v aplikaci ASP.NET Core](xref:security/enforcing-ssl) tématu.</span><span class="sxs-lookup"><span data-stu-id="3466b-130">Require SSL on your site by following the steps in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="3466b-131">Používat SecretManager k uložení tokeny přiřadila zprostředkovatele přihlášení</span><span class="sxs-lookup"><span data-stu-id="3466b-131">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="3466b-132">Přihlášení prostřednictvím sociální sítě poskytovatelů přiřadit **Id aplikace** a **tajný klíč aplikace** tokeny během procesu registrace (přesné pojmenování závisí zprostředkovatele).</span><span class="sxs-lookup"><span data-stu-id="3466b-132">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process (exact naming varies by provider).</span></span>

<span data-ttu-id="3466b-133">Tyto hodnoty jsou efektivně *uživatelské jméno* a *heslo* vaše aplikace používá pro přístup k jejich rozhraní API a tvoří "všechna tajemství" které může být propojený konfiguraci vaší aplikace pomocí **Tajný klíč správce** místo je uložen v konfiguračních souborech přímo nebo přímo v kódu je.</span><span class="sxs-lookup"><span data-stu-id="3466b-133">These values are effectively the *user name* and *password* your application uses to access their API, and constitute the "secrets" that can be linked to your application configuration with the help of **Secret Manager** instead of storing them in configuration files directly or hard-coding them.</span></span>

<span data-ttu-id="3466b-134">Postupujte podle kroků v [bezpečného úložiště tajné klíče aplikace během vývoje v ASP.NET Core](xref:security/app-secrets) téma tak, aby můžete ukládat tokeny přiřadí každé níže zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3466b-134">Follow the steps in [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) topic so that you can store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="3466b-135">Instalační program zprostředkovatele přihlášení, které jsou požadované aplikací</span><span class="sxs-lookup"><span data-stu-id="3466b-135">Setup login providers required by your application</span></span>

<span data-ttu-id="3466b-136">Ke konfiguraci aplikace pomocí příslušného zprostředkovatele použijte v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="3466b-136">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="3466b-137">[Facebook](facebook-logins.md) pokyny</span><span class="sxs-lookup"><span data-stu-id="3466b-137">[Facebook](facebook-logins.md) instructions</span></span>
* <span data-ttu-id="3466b-138">[Twitter](twitter-logins.md) pokyny</span><span class="sxs-lookup"><span data-stu-id="3466b-138">[Twitter](twitter-logins.md) instructions</span></span>
* <span data-ttu-id="3466b-139">[Google](google-logins.md) pokyny</span><span class="sxs-lookup"><span data-stu-id="3466b-139">[Google](google-logins.md) instructions</span></span>
* <span data-ttu-id="3466b-140">[Microsoft](microsoft-logins.md) pokyny</span><span class="sxs-lookup"><span data-stu-id="3466b-140">[Microsoft](microsoft-logins.md) instructions</span></span>
* <span data-ttu-id="3466b-141">[Ostatní poskytovatele](other-logins.md) pokyny</span><span class="sxs-lookup"><span data-stu-id="3466b-141">[Other provider](other-logins.md) instructions</span></span>

## <a name="optionally-set-password"></a><span data-ttu-id="3466b-142">Volitelně můžete nastavit heslo</span><span class="sxs-lookup"><span data-stu-id="3466b-142">Optionally set password</span></span>

<span data-ttu-id="3466b-143">Při registraci prostřednictvím poskytovatele externí přihlášení, není nutné heslo zaregistrována aplikace.</span><span class="sxs-lookup"><span data-stu-id="3466b-143">When you register with an external login provider, you do not have a password registered with the app.</span></span> <span data-ttu-id="3466b-144">To nebude můžete vytvářet a zapamatování hesla pro lokalitu, ale také udržuje je závislá na na externího zprostředkovatele přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3466b-144">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="3466b-145">Pokud není k dispozici na externího zprostředkovatele přihlášení, nebudete moct přihlásit k webu.</span><span class="sxs-lookup"><span data-stu-id="3466b-145">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="3466b-146">Vytvořte heslo a přihlaste se pomocí e-mailu nastavený během v procesu přihlašování s externí zprostředkovatele:</span><span class="sxs-lookup"><span data-stu-id="3466b-146">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="3466b-147">Klepněte na **Hello <email alias>**  v pravém horním rohu přejděte na odkaz **spravovat** zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3466b-147">Tap the **Hello <email alias>** link at the top right corner to navigate to the **Manage** view.</span></span>

![Zobrazení Správa webové aplikace](index/_static/pass1a.png)

* <span data-ttu-id="3466b-149">Klepněte na **vytvořit**</span><span class="sxs-lookup"><span data-stu-id="3466b-149">Tap **Create**</span></span>

![Nastavit stránku heslo](index/_static/pass2a.png)

* <span data-ttu-id="3466b-151">Nastavte platné heslo a tím můžete se přihlásit pomocí e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3466b-151">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3466b-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3466b-152">Next steps</span></span>

* <span data-ttu-id="3466b-153">Tento článek zavedená externího ověřování a vysvětlení nezbytné součásti potřebné k přidání externích přihlášení do aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3466b-153">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="3466b-154">Referenční stránky specifický pro zprostředkovatele konfigurace přihlášení pro zprostředkovatele požadované aplikace.</span><span class="sxs-lookup"><span data-stu-id="3466b-154">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
