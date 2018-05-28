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
ms.openlocfilehash: 7527d3c075fd845ac804d4cfd56469a0679ed7e8
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/17/2018
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a><span data-ttu-id="d3b4c-103">Identita vygenerované uživatelské rozhraní ve projektů ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3b4c-103">Scaffold Identity in ASP.NET Core projects</span></span>

<span data-ttu-id="d3b4c-104">podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d3b4c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d3b4c-105">Poskytuje ASP.NET Core, 2,1 a novější [ASP.NET Core Identity](xref:security/authentication/identity) jako [knihovny tříd Razor](xref:mvc/razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="d3b4c-105">ASP.NET Core 2.1 and later provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:mvc/razor-pages/ui-class).</span></span> <span data-ttu-id="d3b4c-106">Aplikace, které zahrnují Identity můžete použít scaffolder selektivně přidat zdrojový kód obsažené v Identity Razor třídu knihovny (RCL).</span><span class="sxs-lookup"><span data-stu-id="d3b4c-106">Applications that include Identity can apply the scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="d3b4c-107">Můžete chtít generovat zdrojový kód, abyste mohli upravit kód a změnit chování.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-107">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="d3b4c-108">Například může vyzvat scaffolder ke generování kódu použít v registraci.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-108">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="d3b4c-109">Generovaného kódu mají přednost před stejný kód v Identity RCL.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-109">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="d3b4c-110">Aplikace, které provádějí **není** zahrnují ověřování můžete použít k přidání balíčku RCL Identity scaffolder.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-110">Applications that do **not** include authentication can apply the scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="d3b4c-111">Máte možnost výběru Identity kódu má být vygenerován.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-111">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="d3b4c-112">I když scaffolder generuje většinu nezbytného kódu, budete muset aktualizovat projektu dokončí proces.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-112">Although the scaffolder generates most of the necessary code, you'll have to update your project to complete the process.</span></span> <span data-ttu-id="d3b4c-113">Tento dokument popisuje kroky potřebné k dokončení aktualizace Identity generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-113">This document explains the steps needed to complete an Identity scaffolding update.</span></span>

<span data-ttu-id="d3b4c-114">Při spuštění Identity scaffolder *ScaffoldingReadme.txt* soubor je vytvořen v adresáři projektu.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-114">When the Identity scaffolder is run, a *ScaffoldingReadme.txt* file is created in the project directory.</span></span> <span data-ttu-id="d3b4c-115">*ScaffoldingReadme.txt* soubor obsahuje obecné pokyny, co je potřeba provést aktualizaci Identity generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-115">The *ScaffoldingReadme.txt* file contains general instructions on what's need to complete the Identity scaffolding update.</span></span> <span data-ttu-id="d3b4c-116">Tento dokument obsahuje podrobnější pokyny než čtení *ScaffoldingReadme.txt* souboru.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-116">This document contains more complete instructions than the read the *ScaffoldingReadme.txt* file.</span></span>

<span data-ttu-id="d3b4c-117">Doporučujeme používat systému správy zdrojů, které jsou uvedeny rozdíly souborů a umožňuje zpět změny mimo.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-117">We recommend using a source control system that shows file differences and allows you to back out of changes.</span></span> <span data-ttu-id="d3b4c-118">Zkontrolujte změny, po spuštění Identity scaffolder.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-118">Inspect the changes after running the Identity scaffolder.</span></span>

## <a name="scaffold-identity-into-an-empty-project"></a><span data-ttu-id="d3b4c-119">Vygenerované uživatelské rozhraní identity do prázdný projekt</span><span class="sxs-lookup"><span data-stu-id="d3b4c-119">Scaffold identity into an empty project</span></span>

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d3b4c-120">Přidejte následující zvýrazněný volání `Startup` třídy:</span><span class="sxs-lookup"><span data-stu-id="d3b4c-120">Add the following highlighted calls to the `Startup` class:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a><span data-ttu-id="d3b4c-121">Vygenerované uživatelské rozhraní identity do projektu Razor bez existující autorizace</span><span class="sxs-lookup"><span data-stu-id="d3b4c-121">Scaffold identity into a Razor project without existing authorization</span></span>

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0-rc1-final

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d3b4c-122">Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-122">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d3b4c-123">Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d3b4c-123">for more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d3b4c-124">V `Configure` metodu `Startup` třídy, volejte [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d3b4c-124">In the `Configure` method of the `Startup` class, call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a><span data-ttu-id="d3b4c-125">Změny rozložení</span><span class="sxs-lookup"><span data-stu-id="d3b4c-125">Layout changes</span></span>

<span data-ttu-id="d3b4c-126">Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) k rozložení souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b4c-126">Optional: Add the login partial (`_LoginPartial`) to the layout file:</span></span>

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-individual-authorization"></a><span data-ttu-id="d3b4c-127">Vygenerované uživatelské rozhraní identity do projektu Razor s jednotlivé autorizace</span><span class="sxs-lookup"><span data-stu-id="d3b4c-127">Scaffold identity into a Razor project with individual authorization</span></span>

<!--
dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
<span data-ttu-id="d3b4c-128">Některé možnosti Identity konfigurované v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-128">Some Identity options are configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d3b4c-129">Další informace najdete v tématu [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="d3b4c-129">For more information, see [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a><span data-ttu-id="d3b4c-130">Vygenerované uživatelské rozhraní identity do projektu aplikace MVC bez existující autorizace</span><span class="sxs-lookup"><span data-stu-id="d3b4c-130">Scaffold identity into an MVC project without existing authorization</span></span>

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0-rc1-final

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

<span data-ttu-id="d3b4c-131">Volitelné: Přidejte částečné přihlášení (`_LoginPartial`) na *Views/Shared/_Layout.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="d3b4c-131">Optional: Add the login partial (`_LoginPartial`) to the *Views/Shared/_Layout.cshtml* file:</span></span>

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* <span data-ttu-id="d3b4c-132">Přesunout *Pages/Shared/_LoginPartial.cshtml* do souboru *Views/Shared/_LoginPartial.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d3b4c-132">Move the *Pages/Shared/_LoginPartial.cshtml* file to *Views/Shared/_LoginPartial.cshtml*</span></span>

<span data-ttu-id="d3b4c-133">Identita je nastavena v *Areas/Identity/IdentityHostingStartup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-133">Identity is configured in *Areas/Identity/IdentityHostingStartup.cs*.</span></span> <span data-ttu-id="d3b4c-134">Další informace najdete v tématu IHostingStartup.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-134">For more information, see IHostingStartup.</span></span>

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<span data-ttu-id="d3b4c-135">Volání [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) po `UseStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="d3b4c-135">Call [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) after `UseStaticFiles`:</span></span>

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-individual-authorization"></a><span data-ttu-id="d3b4c-136">Vygenerované uživatelské rozhraní identity do projektu aplikace MVC s jednotlivé autorizace</span><span class="sxs-lookup"><span data-stu-id="d3b4c-136">Scaffold identity into an MVC project with individual authorization</span></span>

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

<span data-ttu-id="d3b4c-137">Odstranit *stránky a sdílených* složky a soubory v této složce.</span><span class="sxs-lookup"><span data-stu-id="d3b4c-137">Delete the *Pages/Shared* folder and the files in that folder.</span></span>