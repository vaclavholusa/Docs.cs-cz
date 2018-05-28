---
title: Podpora ochrany nařízení (GDPR) obecné Data v ASP.NET Core
author: rick-anderson
description: Ukazuje, jak přístup k bodům rozšíření GDPR v ASP.NET Core webové aplikace.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: dc1724e8a78c25d3697d14ad784ce853737681f2
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/24/2018
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="b54e1-103">Podpora Evropa obecné Data Protection nařízení (GDPR) v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b54e1-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="b54e1-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b54e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b54e1-105">Základní technologie ASP.NET poskytuje rozhraní API a šablony, které vám pomůžou splňovat některé [UE obecné Data Protection nařízení (GDPR)](https://www.eugdpr.org/) požadavky:</span><span class="sxs-lookup"><span data-stu-id="b54e1-105">ASP.NET Core provides APIs and templates to help meet some of the [UE General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="b54e1-106">Šablony projektů zahrnují body rozšíření a provizorní značek, které můžete nahradit zásady používání souborů cookie a osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="b54e1-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="b54e1-107">Funkce souhlasu soubor cookie umožňuje požádat o (a sledovat) souhlasu od uživatelů pro ukládání osobní údaje.</span><span class="sxs-lookup"><span data-stu-id="b54e1-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="b54e1-108">Pokud uživatel nedala souhlas shromažďování dat a aplikace je nastaven s [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) k `true`, nebude odeslána nepotřebných souborů cookie v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="b54e1-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="b54e1-109">Soubory cookie může být označen jako nezbytné.</span><span class="sxs-lookup"><span data-stu-id="b54e1-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="b54e1-110">Základní soubory cookie jsou odesílány do prohlížeče, i v případě, že nedala souhlas uživatele a sledování je zakázaná.</span><span class="sxs-lookup"><span data-stu-id="b54e1-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="b54e1-111">[Soubory cookie TempData a relace](#tempdata) nejsou funkční, když je zakázáno sledování.</span><span class="sxs-lookup"><span data-stu-id="b54e1-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="b54e1-112">[Spravovat Identity](#pd) stránka obsahuje odkaz na stažení a odstranění dat uživatele.</span><span class="sxs-lookup"><span data-stu-id="b54e1-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="b54e1-113">[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) můžete testovat většinu GDPR rozšíření bodů a přidat do šablony ASP.NET Core 2.1 rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="b54e1-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="b54e1-114">Najdete v článku [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) soubor pro testování pokyny.</span><span class="sxs-lookup"><span data-stu-id="b54e1-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="b54e1-115">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b54e1-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="b54e1-116">Podpora GDPR ASP.NET Core v šabloně generovaného kódu</span><span class="sxs-lookup"><span data-stu-id="b54e1-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="b54e1-117">Stránky Razor a MVC projekty vytvořené pomocí šablony projektu zahrnují následující podporu GDPR:</span><span class="sxs-lookup"><span data-stu-id="b54e1-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="b54e1-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) a [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) se nastavují v `Startup`.</span><span class="sxs-lookup"><span data-stu-id="b54e1-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="b54e1-119">*_CookieConsentPartial.cshtml* [částečné zobrazení](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="b54e1-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="b54e1-120">*Pages/Privacy.cshtml* nebo *Home/rivacy.cshtml* zobrazení stránky podrobností zásady ochrany osobních údajů pro váš web poskytuje.</span><span class="sxs-lookup"><span data-stu-id="b54e1-120">The *Pages/Privacy.cshtml*  or *Home/rivacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="b54e1-121">*_CookieConsentPartial.cshtml* souboru generuje odkaz na stránce o ochraně osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="b54e1-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="b54e1-122">Pro aplikace vytvořené s jednotlivých uživatelských účtů, spravovat stránka obsahuje odkazy na stažení a odstranění [osobních uživatelských dat](#pd).</span><span class="sxs-lookup"><span data-stu-id="b54e1-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="b54e1-123">CookiePolicyOptions a UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="b54e1-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="b54e1-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) jsou v inicializovat `Startup` třída `ConfigureServices` metoda:</span><span class="sxs-lookup"><span data-stu-id="b54e1-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="b54e1-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) je volána `Startup` třída `Configure` metoda:</span><span class="sxs-lookup"><span data-stu-id="b54e1-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="b54e1-126">_CookieConsentPartial.cshtml částečné zobrazení</span><span class="sxs-lookup"><span data-stu-id="b54e1-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="b54e1-127">*_CookieConsentPartial.cshtml* částečné zobrazení:</span><span class="sxs-lookup"><span data-stu-id="b54e1-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="b54e1-128">Toto částečné:</span><span class="sxs-lookup"><span data-stu-id="b54e1-128">This partial:</span></span>

* <span data-ttu-id="b54e1-129">Získá stav sledování pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="b54e1-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="b54e1-130">Pokud aplikace je nakonfigurovaná tak, aby vyžadovala souhlasu musí uživatel souhlasit, než lze sledovat soubory cookie.</span><span class="sxs-lookup"><span data-stu-id="b54e1-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="b54e1-131">Pokud se vyžaduje souhlas, chrome souhlasu souboru cookie vyřešen nad vytvořené v navigačním panelu *Pages/Shared/_Layout.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="b54e1-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="b54e1-132">Poskytuje HTML `<p>` element pro shrnutí ochrany osobních údajů a souborů cookie použít zásady.</span><span class="sxs-lookup"><span data-stu-id="b54e1-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="b54e1-133">Obsahuje odkaz na *Pages/Privacy.cshtml* kde můžete podrobností zásady ochrany osobních údajů vaší lokality.</span><span class="sxs-lookup"><span data-stu-id="b54e1-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="b54e1-134">Základní soubory cookie</span><span class="sxs-lookup"><span data-stu-id="b54e1-134">Essential cookies</span></span>

<span data-ttu-id="b54e1-135">Pokud nebylo uděleno souhlasu, jenom soubory cookie označena nezbytné odešlou do prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b54e1-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="b54e1-136">Následující kód vytvoří soubor cookie základní:</span><span class="sxs-lookup"><span data-stu-id="b54e1-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="b54e1-137">Soubory cookie stav Tempdata zprostředkovatele a relace nejsou nezbytně nutné</span><span class="sxs-lookup"><span data-stu-id="b54e1-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="b54e1-138">[Tempdata zprostředkovatele](xref:fundamentals/app-state#tempdata) souboru cookie, není podstatné.</span><span class="sxs-lookup"><span data-stu-id="b54e1-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="b54e1-139">Pokud sledování je zakázán, zprostředkovatel Tempdata není funkční.</span><span class="sxs-lookup"><span data-stu-id="b54e1-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="b54e1-140">Povolit zprostředkovatele Tempdata, když je zakázáno sledování, označte TempData soubor cookie jako důležité `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b54e1-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="b54e1-141">[Stav relace](xref:fundamentals/app-state) souborů cookie nejsou nezbytně nutné.</span><span class="sxs-lookup"><span data-stu-id="b54e1-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="b54e1-142">Stav relace není funkční, když je zakázáno sledování.</span><span class="sxs-lookup"><span data-stu-id="b54e1-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="b54e1-143">Osobní data</span><span class="sxs-lookup"><span data-stu-id="b54e1-143">Personal data</span></span>

<span data-ttu-id="b54e1-144">ASP.NET Core aplikací vytvořených v jednotlivých uživatelských účtů zahrnují kód pro stažení a odstranění osobní data.</span><span class="sxs-lookup"><span data-stu-id="b54e1-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="b54e1-145">Vyberte uživatelské jméno a potom vyberte **osobní data**:</span><span class="sxs-lookup"><span data-stu-id="b54e1-145">Select the user name and then select **Personal data**:</span></span>

![Spravovat osobní data stránky](gdpr/_static/pd.png)

<span data-ttu-id="b54e1-147">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="b54e1-147">Notes:</span></span>

* <span data-ttu-id="b54e1-148">Ke generování `Account/Manage` kódu, najdete v části [Identity vygenerované uživatelské rozhraní](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="b54e1-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="b54e1-149">Odstraňte a stáhnout pouze dopad výchozí identifikační údaje.</span><span class="sxs-lookup"><span data-stu-id="b54e1-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="b54e1-150">Aplikace vytvořit vlastní uživatelská data musí být rozšířené na odstranění nebo stahování dat vlastní uživatele.</span><span class="sxs-lookup"><span data-stu-id="b54e1-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="b54e1-151">Problém Githubu [postup přidání nebo odstranění vlastní uživatelská data na identitu](https://github.com/aspnet/Docs/issues/6226) sleduje navrhované článku o vytvoření vlastní/odstranění/stahování vlastní uživatelská data.</span><span class="sxs-lookup"><span data-stu-id="b54e1-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="b54e1-152">Pokud chcete tohoto tématu nastavovat, ponechejte úspěch reakce na problém.</span><span class="sxs-lookup"><span data-stu-id="b54e1-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="b54e1-153">Uložit tokeny pro daného uživatele, které jsou uložené v tabulce databáze Identity `AspNetUserTokens` se odstraní při odstranění uživatele prostřednictvím kaskádové odstranění chování kvůli [cizí klíč](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="b54e1-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="b54e1-154">Šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="b54e1-154">Encryption at rest</span></span>

<span data-ttu-id="b54e1-155">Některé databáze a úložiště mechanismy umožňují šifrování v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="b54e1-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="b54e1-156">Šifrování v klidovém stavu:</span><span class="sxs-lookup"><span data-stu-id="b54e1-156">Encryption at rest:</span></span>

* <span data-ttu-id="b54e1-157">Automaticky šifruje uložená data.</span><span class="sxs-lookup"><span data-stu-id="b54e1-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="b54e1-158">Šifruje bez konfigurace, programování nebo jiné pracovní pro software, který přistupuje k datům.</span><span class="sxs-lookup"><span data-stu-id="b54e1-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="b54e1-159">Je nejjednodušší a nejbezpečnější možnost.</span><span class="sxs-lookup"><span data-stu-id="b54e1-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="b54e1-160">Umožňuje spravovat klíče a šifrování databáze.</span><span class="sxs-lookup"><span data-stu-id="b54e1-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="b54e1-161">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b54e1-161">For example:</span></span>

* <span data-ttu-id="b54e1-162">Microsoft SQL a Azure SQL poskytují [transparentní šifrování dat](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="b54e1-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="b54e1-163">SQL Azure ve výchozím nastavení šifruje databáze</span><span class="sxs-lookup"><span data-stu-id="b54e1-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="b54e1-164">[Ve výchozím nastavení jsou zašifrované Azure BLOB, soubory, Table a Queue Storage](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="b54e1-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="b54e1-165">Pro databáze, které neposkytují integrované šifrování v klidovém stavu nebudete moct pomocí šifrování disku lze zadat stejnou úroveň ochrany.</span><span class="sxs-lookup"><span data-stu-id="b54e1-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="b54e1-166">Příklad:</span><span class="sxs-lookup"><span data-stu-id="b54e1-166">For example:</span></span>

* [<span data-ttu-id="b54e1-167">Nástroj BitLocker pro systém windows server</span><span class="sxs-lookup"><span data-stu-id="b54e1-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="b54e1-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="b54e1-168">Linux:</span></span>
  * [<span data-ttu-id="b54e1-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="b54e1-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="b54e1-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="b54e1-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b54e1-171">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="b54e1-171">Additional Resources</span></span>

* [<span data-ttu-id="b54e1-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="b54e1-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)