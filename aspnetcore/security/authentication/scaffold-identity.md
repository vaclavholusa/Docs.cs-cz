---
title: Identita vygenerované uživatelské rozhraní ve projektů ASP.NET Core
author: rick-anderson
description: Zjistěte, jak chcete vygenerovat Identity v projektu ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: 80cd39af61e856d3ce92db1c26e70788bcdca83d
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/18/2018
ms.locfileid: "35725816"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="9e83b-103">Identita vygenerované uživatelské rozhraní ve projektů ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e83b-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="9e83b-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9e83b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9e83b-105">Poskytuje ASP.NET Core, 2,1 a novější [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="9e83b-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="9e83b-106">Aplikace, které zahrnují Identity můžete použít scaffolder selektivně přidat zdrojový kód obsažené v Identity Razor třídu knihovny (RCL).</span><span class="sxs-lookup"><span data-stu-id="9e83b-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="9e83b-107">Můžete chtít generovat zdrojový kód, abyste mohli upravit kód a změnit chování.</span><span class="sxs-lookup"><span data-stu-id="9e83b-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="9e83b-108">Například může vyzvat scaffolder ke generování kódu použít v registraci.</span><span class="sxs-lookup"><span data-stu-id="9e83b-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="9e83b-109">Generovaného kódu mají přednost před stejný kód v Identity RCL.</span><span class="sxs-lookup"><span data-stu-id="9e83b-109">Generated code takes precedence over the same code in the Identity RCL.</span></span> <span data-ttu-id="9e83b-110">Pokud chcete získat úplné řízení pro uživatelské rozhraní a nechcete použít výchozí RCL, najdete v části [vytvořit úplné identity uživatelského rozhraní zdroj](#full).</span><span class="sxs-lookup"><span data-stu-id="9e83b-110">To gain full control of the UI and not use the default RCL, see the section [Create full identity UI source](#full).</span></span>

<span data-ttu-id="9e83b-111">Aplikace, které provádějí **není** zahrnují ověřování můžete použít k přidání balíčku RCL Identity scaffolder.</span><span class="sxs-lookup"><span data-stu-id="9e83b-111">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="9e83b-112">Máte možnost výběru Identity kódu má být vygenerován.</span><span class="sxs-lookup"><span data-stu-id="9e83b-112">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="9e83b-113">I když scaffolder generuje většinu nezbytného kódu, budete muset aktualizovat projektu dokončí proces.</span><span class="sxs-lookup"><span data-stu-id="9e83b-113">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="9e83b-114">Tento dokument popisuje kroky potřebné k dokončení aktualizace Identity generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9e83b-114">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="9e83b-115">Při spuštění Identity scaffolder *ScaffoldingReadme.txt* soubor je vytvořen v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="9e83b-115">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="9e83b-116">*ScaffoldingReadme.txt* soubor obsahuje obecné pokyny, co je potřeba k dokončení aktualizace Identity generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="9e83b-116">The *ScaffoldingReadme.txt* file contains general instructions on what's needed to complete the Identity scaffolding update.</span></span> <span data-ttu-id="9e83b-117">Tento dokument obsahuje podrobnější pokyny, než *ScaffoldingReadme.txt* souboru.</span><span class="sxs-lookup"><span data-stu-id="9e83b-117">This document contains more complete instructions than the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="9e83b-118">Doporučujeme používat systému správy zdrojů, které jsou uvedeny rozdíly souborů a umožňuje zpět změny mimo.</span><span class="sxs-lookup"><span data-stu-id="9e83b-118">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="9e83b-119">Zkontrolujte změny, po spuštění Identity scaffolder.</span><span class="sxs-lookup"><span data-stu-id="9e83b-119">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="9e83b-120">Vygenerované uživatelské rozhraní identity do prázdný projekt</span><span class="sxs-lookup"><span data-stu-id="9e83b-120">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="9e83b-121">Přidejte následující zvýrazněný volání `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="9e83b-121">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="9e83b-122">Vygenerované uživatelské rozhraní identity do projektu Razor bez existující autorizace</span><span class="sxs-lookup"><span data-stu-id="9e83b-122">Scaffold identity into a Razor project without existing authorization</span></span>

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

<span data-ttu-id="9e83b-123">Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e83b-123">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="9e83b-124">Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="9e83b-124">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a><span data-ttu-id="9e83b-125">UseAuthentication, migrace a rozložení</span><span class="sxs-lookup"><span data-stu-id="9e83b-125">Migrations, UseAuthentication, and layout</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="9e83b-126">V `Configure` metodu `Startup` třídy, volejte [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="9e83b-126">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="9e83b-127">Změny rozložení</span><span class="sxs-lookup"><span data-stu-id="9e83b-127">Layout changes</span></span>

<span data-ttu-id="9e83b-128">Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) k rozložení souboru:</span><span class="sxs-lookup"><span data-stu-id="9e83b-128">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a><span data-ttu-id="9e83b-129">Vygenerované uživatelské rozhraní identity do projektu Razor s autorizace</span><span class="sxs-lookup"><span data-stu-id="9e83b-129">Scaffold identity into a Razor project with authorization</span></span>

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
<span data-ttu-id="9e83b-130">Některé možnosti Identity konfigurované v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e83b-130">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="9e83b-131">Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="9e83b-131">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="9e83b-132">Vygenerované uživatelské rozhraní identity do projektu aplikace MVC bez existující autorizace</span><span class="sxs-lookup"><span data-stu-id="9e83b-132">Scaffold identity into an MVC project without existing authorization</span></span>

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

<span data-ttu-id="9e83b-133">Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) na *Views/Shared/_Layout.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="9e83b-133">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="9e83b-134">Přesunout *Pages/Shared/_LoginPartial.cshtml* do souboru *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9e83b-134">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="9e83b-135">Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9e83b-135">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="9e83b-136">Další informace najdete v tématu IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="9e83b-136">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="9e83b-137">Volání [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="9e83b-137">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a><span data-ttu-id="9e83b-138">Vygenerované uživatelské rozhraní identity do projektu aplikace MVC s autorizace</span><span class="sxs-lookup"><span data-stu-id="9e83b-138">Scaffold identity into an MVC project with authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="9e83b-139">Odstranit *stránky a sdílených* složky a soubory v této složce.</span><span class="sxs-lookup"><span data-stu-id="9e83b-139">Delete the *Pages/Shared* folder and the files in that folder.</span></span>

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a><span data-ttu-id="9e83b-140">Vytvořte zdroj úplné identity uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="9e83b-140">Create full identity UI source</span></span>

<span data-ttu-id="9e83b-141">Pokud chcete zachovat plnou kontrolu nad rozhraní Identity, spusťte Identity scaffolder a vyberte **přepsat všechny soubory**.</span><span class="sxs-lookup"><span data-stu-id="9e83b-141">To maintain full control of the Identity UI, run the Identity scaffolder and select **Override all files**.</span></span>

<span data-ttu-id="9e83b-142">Následující zvýrazněný kód ukazuje změny výchozího uživatelského rozhraní Identity nahraďte Identity ve webové aplikaci ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="9e83b-142">The following highlighted code shows the changes to replace the default Identity UI with Identity in an ASP.NET Core 2.1 web app.</span></span> <span data-ttu-id="9e83b-143">Můžete k tomu má plnou kontrolu nad rozhraní Identity.</span><span class="sxs-lookup"><span data-stu-id="9e83b-143">You might want to do this to have full control of the Identity UI.</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

<span data-ttu-id="9e83b-144">Výchozí hodnota Identity je nahrazena v následujícím kódu: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="9e83b-144">The default Identity is replaced in the following code: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet2)]</span></span>

<span data-ttu-id="9e83b-145">Následující kód konfiguruje ASP.NET Core autorizovat Identity stránek, které vyžadují autorizace: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="9e83b-145">The following code configures ASP.NET Core to authorize the Identity pages that require authorization: [!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]</span></span>

<span data-ttu-id="9e83b-146">Následující kód nastaví soubor cookie Identity používat správnou cestu stránky Identity.</span><span class="sxs-lookup"><span data-stu-id="9e83b-146">The following the code sets the Identity cookie to use the correct Identity pages path.</span></span>
[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

<span data-ttu-id="9e83b-147">Zaregistrovat `IEmailSender` implementace, například:</span><span class="sxs-lookup"><span data-stu-id="9e83b-147">Register an `IEmailSender` implementation, for example:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupFull.cs?name=snippet4)]