---
title: Identita vygenerované uživatelské rozhraní ve projektů ASP.NET Core
author: rick-anderson
description: Zjistěte, jak chcete vygenerovat Identity v projektu ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: cf6544d8b671f026c8466fa8dff506027b64cf1f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276315"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identita vygenerované uživatelské rozhraní ve projektů ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Poskytuje ASP.NET Core, 2,1 a novější [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:razor-pages/ui-class). Aplikace, které zahrnují Identity můžete použít scaffolder selektivně přidat zdrojový kód obsažené v Identity Razor třídu knihovny (RCL). Můžete chtít generovat zdrojový kód, abyste mohli upravit kód a změnit chování. Například může vyzvat scaffolder ke generování kódu použít v registraci. Generovaného kódu mají přednost před stejný kód v Identity RCL. Pokud chcete získat úplné řízení pro uživatelské rozhraní a nechcete použít výchozí RCL, najdete v části [vytvořit úplné identity uživatelského rozhraní zdroj](#full).

Aplikace, které provádějí **není** zahrnují ověřování můžete použít k přidání balíčku RCL Identity scaffolder. Máte možnost výběru Identity kódu má být vygenerován.

I když scaffolder generuje většinu nezbytného kódu, budete muset aktualizovat projektu dokončí proces. Tento dokument popisuje kroky potřebné k dokončení aktualizace Identity generování uživatelského rozhraní.

Při spuštění Identity scaffolder *ScaffoldingReadme.txt* soubor je vytvořen v adresáři projektu. *ScaffoldingReadme.txt* soubor obsahuje obecné pokyny, co je potřeba k dokončení aktualizace Identity generování uživatelského rozhraní. Tento dokument obsahuje podrobnější pokyny, než *ScaffoldingReadme.txt* souboru.

Doporučujeme používat systému správy zdrojů, které jsou uvedeny rozdíly souborů a umožňuje zpět změny mimo. Zkontrolujte změny, po spuštění Identity scaffolder.

## <a name="scaffold-identity-into-an-empty-project"></a>Vygenerované uživatelské rozhraní identity do prázdný projekt

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Přidejte následující zvýrazněný volání `Startup` třídy:

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Vygenerované uživatelské rozhraní identity do projektu Razor bez existující autorizace

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*. Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>UseAuthentication, migrace a rozložení

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

V `Configure` metodu `Startup` třídy, volejte [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Změny rozložení

Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) k rozložení souboru:

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Vygenerované uživatelské rozhraní identity do projektu Razor s autorizace

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Některé možnosti Identity konfigurované v *Areas/Identity/IdentityHostingStartup.cs*. Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Vygenerované uživatelské rozhraní identity do projektu aplikace MVC bez existující autorizace

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) na *Views/Shared/_Layout.cshtml* souboru:

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Přesunout *Pages/Shared/_LoginPartial.cshtml* do souboru *Views/Shared/_LoginPartial.cshtml*

Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*. Další informace najdete v tématu IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Volání [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Vygenerované uživatelské rozhraní identity do projektu aplikace MVC s autorizace

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Odstranit *stránky a sdílených* složky a soubory v této složce.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Vytvořte zdroj úplné identity uživatelského rozhraní

Pokud chcete zachovat plnou kontrolu nad rozhraní Identity, spusťte Identity scaffolder a vyberte **přepsat všechny soubory**.

Následující zvýrazněný kód ukazuje změny výchozího uživatelského rozhraní Identity nahraďte Identity ve webové aplikaci ASP.NET Core 2.1. Můžete k tomu má plnou kontrolu nad rozhraní Identity.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

Výchozí hodnota Identity je nahrazena v následujícím kódu:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Následující sady kódu [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), a [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Zaregistrovat `IEmailSender` implementace, například:

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
