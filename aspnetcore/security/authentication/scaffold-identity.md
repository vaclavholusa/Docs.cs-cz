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
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="bb9c3-103">Identita vygenerované uživatelské rozhraní ve projektů ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb9c3-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="bb9c3-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bb9c3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bb9c3-105">Poskytuje ASP.NET Core, 2,1 a novější [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="bb9c3-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="bb9c3-106">Aplikace, které zahrnují Identity můžete použít scaffolder selektivně přidat zdrojový kód obsažené v Identity Razor třídu knihovny (RCL).</span><span class="sxs-lookup"><span data-stu-id="bb9c3-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="bb9c3-107">Můžete chtít generovat zdrojový kód, abyste mohli upravit kód a změnit chování.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="bb9c3-108">Například může vyzvat scaffolder ke generování kódu použít v registraci.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="bb9c3-109">Generovaného kódu mají přednost před stejný kód v Identity RCL.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="bb9c3-110">Pokud chcete získat úplné řízení pro uživatelské rozhraní a nechcete použít výchozí RCL, najdete v části [vytvořit úplné identity uživatelského rozhraní zdroj](#full).</span><span class="sxs-lookup"><span data-stu-id="bb9c3-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="bb9c3-111">Aplikace, které provádějí **není** zahrnují ověřování můžete použít k přidání balíčku RCL Identity scaffolder.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="bb9c3-112">Máte možnost výběru Identity kódu má být vygenerován.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="bb9c3-113">I když scaffolder generuje většinu nezbytného kódu, budete muset aktualizovat projektu dokončí proces.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="bb9c3-114">Tento dokument popisuje kroky potřebné k dokončení aktualizace Identity generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="bb9c3-115">Při spuštění Identity scaffolder *ScaffoldingReadme.txt* soubor je vytvořen v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="bb9c3-116">*ScaffoldingReadme.txt* soubor obsahuje obecné pokyny, co je potřeba k dokončení aktualizace Identity generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="bb9c3-117">Tento dokument obsahuje podrobnější pokyny, než *ScaffoldingReadme.txt* souboru.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="bb9c3-118">Doporučujeme používat systému správy zdrojů, které jsou uvedeny rozdíly souborů a umožňuje zpět změny mimo.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="bb9c3-119">Zkontrolujte změny, po spuštění Identity scaffolder.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="bb9c3-120">Vygenerované uživatelské rozhraní identity do prázdný projekt</span><span class="sxs-lookup"><span data-stu-id="bb9c3-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="bb9c3-121">Přidejte následující zvýrazněný volání `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="bb9c3-122">Vygenerované uživatelské rozhraní identity do projektu Razor bez existující autorizace</span><span class="sxs-lookup"><span data-stu-id="bb9c3-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="bb9c3-123">Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="bb9c3-124">Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="bb9c3-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="bb9c3-125">UseAuthentication, migrace a rozložení</span><span class="sxs-lookup"><span data-stu-id="bb9c3-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="bb9c3-126">V `Configure` metodu `Startup` třídy, volejte [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="bb9c3-127">Změny rozložení</span><span class="sxs-lookup"><span data-stu-id="bb9c3-127">Layout changes</span></span>

<span data-ttu-id="bb9c3-128">Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) k rozložení souboru:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="bb9c3-129">Vygenerované uživatelské rozhraní identity do projektu Razor s autorizace</span><span class="sxs-lookup"><span data-stu-id="bb9c3-129">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="bb9c3-130">Některé možnosti Identity konfigurované v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="bb9c3-131">Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="bb9c3-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="bb9c3-132">Vygenerované uživatelské rozhraní identity do projektu aplikace MVC bez existující autorizace</span><span class="sxs-lookup"><span data-stu-id="bb9c3-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="bb9c3-133">Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) na *Views/Shared/_Layout.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="bb9c3-134">Přesunout *Pages/Shared/_LoginPartial.cshtml* do souboru *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb9c3-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="bb9c3-135">Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="bb9c3-136">Další informace najdete v tématu IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="bb9c3-137">Volání [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="bb9c3-138">Vygenerované uživatelské rozhraní identity do projektu aplikace MVC s autorizace</span><span class="sxs-lookup"><span data-stu-id="bb9c3-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="bb9c3-139">Odstranit *stránky a sdílených* složky a soubory v této složce.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="bb9c3-140">Vytvořte zdroj úplné identity uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="bb9c3-140">Create full identity UI source</span></span>

<span data-ttu-id="bb9c3-141">Pokud chcete zachovat plnou kontrolu nad rozhraní Identity, spusťte Identity scaffolder a vyberte **přepsat všechny soubory**.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="bb9c3-142">Následující zvýrazněný kód ukazuje změny výchozího uživatelského rozhraní Identity nahraďte Identity ve webové aplikaci ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="bb9c3-143">Můžete k tomu má plnou kontrolu nad rozhraní Identity.</span><span class="sxs-lookup"><span data-stu-id="bb9c3-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="bb9c3-144">Výchozí hodnota Identity je nahrazena v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-144">The default Identity is replaced in the following code:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

<span data-ttu-id="bb9c3-145">Následující sady kódu [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), a [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span><span class="sxs-lookup"><span data-stu-id="bb9c3-145">The following the code sets the [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), and [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="bb9c3-146">Zaregistrovat `IEmailSender` implementace, například:</span><span class="sxs-lookup"><span data-stu-id="bb9c3-146">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
