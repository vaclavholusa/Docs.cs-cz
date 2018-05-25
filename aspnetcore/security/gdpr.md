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
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Podpora Evropa obecné Data Protection nařízení (GDPR) v ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Základní technologie ASP.NET poskytuje rozhraní API a šablony, které vám pomůžou splňovat některé [UE obecné Data Protection nařízení (GDPR)](https://www.eugdpr.org/) požadavky:

* Šablony projektů zahrnují body rozšíření a provizorní značek, které můžete nahradit zásady používání souborů cookie a osobních údajů.
* Funkce souhlasu soubor cookie umožňuje požádat o (a sledovat) souhlasu od uživatelů pro ukládání osobní údaje. Pokud uživatel nedala souhlas shromažďování dat a aplikace je nastaven s [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) k `true`, nebude odeslána nepotřebných souborů cookie v prohlížeči.
* Soubory cookie může být označen jako nezbytné. Základní soubory cookie jsou odesílány do prohlížeče, i v případě, že nedala souhlas uživatele a sledování je zakázaná.
* [Soubory cookie TempData a relace](#tempdata) nejsou funkční, když je zakázáno sledování.
* [Spravovat Identity](#pd) stránka obsahuje odkaz na stažení a odstranění dat uživatele.

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) můžete testovat většinu GDPR rozšíření bodů a přidat do šablony ASP.NET Core 2.1 rozhraní API. Najdete v článku [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) soubor pro testování pokyny.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Podpora GDPR ASP.NET Core v šabloně generovaného kódu

Stránky Razor a MVC projekty vytvořené pomocí šablony projektu zahrnují následující podporu GDPR:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) a [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) se nastavují v `Startup`.
* *_CookieConsentPartial.cshtml* [částečné zobrazení](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* *Pages/Privacy.cshtml* nebo *Home/rivacy.cshtml* zobrazení stránky podrobností zásady ochrany osobních údajů pro váš web poskytuje. *_CookieConsentPartial.cshtml* souboru generuje odkaz na stránce o ochraně osobních údajů.
* Pro aplikace vytvořené s jednotlivých uživatelských účtů, spravovat stránka obsahuje odkazy na stažení a odstranění [osobních uživatelských dat](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions a UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) jsou v inicializovat `Startup` třída `ConfigureServices` metoda:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) je volána `Startup` třída `Configure` metoda:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>_CookieConsentPartial.cshtml částečné zobrazení

*_CookieConsentPartial.cshtml* částečné zobrazení:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Toto částečné:

* Získá stav sledování pro uživatele. Pokud aplikace je nakonfigurovaná tak, aby vyžadovala souhlasu musí uživatel souhlasit, než lze sledovat soubory cookie. Pokud se vyžaduje souhlas, chrome souhlasu souboru cookie vyřešen nad vytvořené v navigačním panelu *Pages/Shared/_Layout.cshtml* souboru.
* Poskytuje HTML `<p>` element pro shrnutí ochrany osobních údajů a souborů cookie použít zásady.
* Obsahuje odkaz na *Pages/Privacy.cshtml* kde můžete podrobností zásady ochrany osobních údajů vaší lokality.

## <a name="essential-cookies"></a>Základní soubory cookie

Pokud nebylo uděleno souhlasu, jenom soubory cookie označena nezbytné odešlou do prohlížeče. Následující kód vytvoří soubor cookie základní:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Soubory cookie stav Tempdata zprostředkovatele a relace nejsou nezbytně nutné

[Tempdata zprostředkovatele](xref:fundamentals/app-state#tempdata) souboru cookie, není podstatné. Pokud sledování je zakázán, zprostředkovatel Tempdata není funkční. Povolit zprostředkovatele Tempdata, když je zakázáno sledování, označte TempData soubor cookie jako důležité `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Stav relace](xref:fundamentals/app-state) souborů cookie nejsou nezbytně nutné. Stav relace není funkční, když je zakázáno sledování.

<a name="pd"></a>

## <a name="personal-data"></a>Osobní data

ASP.NET Core aplikací vytvořených v jednotlivých uživatelských účtů zahrnují kód pro stažení a odstranění osobní data.

Vyberte uživatelské jméno a potom vyberte **osobní data**:

![Spravovat osobní data stránky](gdpr/_static/pd.png)

Poznámky:

* Ke generování `Account/Manage` kódu, najdete v části [Identity vygenerované uživatelské rozhraní](xref:security/authentication/scaffold-identity).
* Odstraňte a stáhnout pouze dopad výchozí identifikační údaje. Aplikace vytvořit vlastní uživatelská data musí být rozšířené na odstranění nebo stahování dat vlastní uživatele. Problém Githubu [postup přidání nebo odstranění vlastní uživatelská data na identitu](https://github.com/aspnet/Docs/issues/6226) sleduje navrhované článku o vytvoření vlastní/odstranění/stahování vlastní uživatelská data. Pokud chcete tohoto tématu nastavovat, ponechejte úspěch reakce na problém.
* Uložit tokeny pro daného uživatele, které jsou uložené v tabulce databáze Identity `AspNetUserTokens` se odstraní při odstranění uživatele prostřednictvím kaskádové odstranění chování kvůli [cizí klíč](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Šifrování v klidovém stavu

Některé databáze a úložiště mechanismy umožňují šifrování v klidovém stavu. Šifrování v klidovém stavu:

* Automaticky šifruje uložená data.
* Šifruje bez konfigurace, programování nebo jiné pracovní pro software, který přistupuje k datům.
* Je nejjednodušší a nejbezpečnější možnost.
* Umožňuje spravovat klíče a šifrování databáze.

Příklad:

* Microsoft SQL a Azure SQL poskytují [transparentní šifrování dat](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).
* [SQL Azure ve výchozím nastavení šifruje databáze](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Ve výchozím nastavení jsou zašifrované Azure BLOB, soubory, Table a Queue Storage](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Pro databáze, které neposkytují integrované šifrování v klidovém stavu nebudete moct pomocí šifrování disku lze zadat stejnou úroveň ochrany. Příklad:

* [Nástroj BitLocker pro systém windows server](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Další prostředky

* [Microsoft.com/GDPR](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
