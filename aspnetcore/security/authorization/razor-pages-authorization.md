---
title: Vytváření názvů autorizace stránek Razor v ASP.NET Core
author: guardrex
description: Zjistěte, jak řídit přístup ke stránkám s vytváření názvů, které uživateli uživatele autorizovat a umožňoval anonymním uživatelům přístup k stránky nebo složky stránek.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753324"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="bbca8-103">Vytváření názvů autorizace stránek Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bbca8-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="bbca8-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bbca8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bbca8-105">Jedním ze způsobů řízení přístupu v aplikaci pro stránky Razor je použití autorizace konvence při spuštění.</span><span class="sxs-lookup"><span data-stu-id="bbca8-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="bbca8-106">Tato konvence umožňují uživateli uživatele autorizovat a umožňoval anonymním uživatelům přístup k jednotlivé stránky nebo složky stránek.</span><span class="sxs-lookup"><span data-stu-id="bbca8-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="bbca8-107">Platí zásady popsané v tomto tématu automaticky [filtry autorizace](xref:mvc/controllers/filters#authorization-filters) pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="bbca8-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="bbca8-108">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bbca8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bbca8-109">Tato ukázková aplikace používá [ověřování souborem Cookie bez ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="bbca8-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="bbca8-110">Uživatelský účet pro hypotetické uživatele Marie Rodriguez, je pevně zakódované do aplikace.</span><span class="sxs-lookup"><span data-stu-id="bbca8-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="bbca8-111">Použití uživatelského jména e-mailu "maria.rodriguez@contoso.com" a jakékoli heslo k přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="bbca8-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="bbca8-112">Ověření uživatele v `AuthenticateUser` metodu *Pages/Account/Login.cshtml.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="bbca8-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="bbca8-113">V reálný příklad uživatel by ověřovány proti databázi.</span><span class="sxs-lookup"><span data-stu-id="bbca8-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="bbca8-114">ASP.NET Core Identity, postupujte podle pokynů v [Úvod do Identity v ASP.NET Core](xref:security/authentication/identity) tématu.</span><span class="sxs-lookup"><span data-stu-id="bbca8-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="bbca8-115">Koncepty a příkladů uvedených v tomto tématu platí stejně pro aplikace, které používají technologii ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="bbca8-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="bbca8-116">Vyžaduje povolení ke stránce</span><span class="sxs-lookup"><span data-stu-id="bbca8-116">Require authorization to access a page</span></span>

<span data-ttu-id="bbca8-117">Použití [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stránku v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="bbca8-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="bbca8-118">Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor bez rozšíření a který obsahuje pouze lomítka.</span><span class="sxs-lookup"><span data-stu-id="bbca8-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="bbca8-119">[AuthorizePage přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="bbca8-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="bbca8-120">`AuthorizeFilter` Lze použít u třídy modelu stránky s `[Authorize]` atribut filtru.</span><span class="sxs-lookup"><span data-stu-id="bbca8-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="bbca8-121">Další informace najdete v tématu [atribut filtru Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="bbca8-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="bbca8-122">Vyžaduje autorizaci pro přístup ke složce stránek</span><span class="sxs-lookup"><span data-stu-id="bbca8-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="bbca8-123">Použití [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na všechny stránky ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="bbca8-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="bbca8-124">Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="bbca8-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="bbca8-125">[AuthorizeFolder přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="bbca8-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="bbca8-126">Vyžaduje autorizaci pro přístup k stránku oblasti</span><span class="sxs-lookup"><span data-stu-id="bbca8-126">Require authorization to access an area page</span></span>

<span data-ttu-id="bbca8-127">Použití [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) stránku oblasti v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="bbca8-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="bbca8-128">Název stránky je cesta k souboru bez přípony vzhledem k stránky kořenový adresář pro zadanou oblast.</span><span class="sxs-lookup"><span data-stu-id="bbca8-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="bbca8-129">Například název stránky pro soubor *Areas/Identity/Pages/Manage/Accounts.cshtml* je */Správa nebo účty*.</span><span class="sxs-lookup"><span data-stu-id="bbca8-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="bbca8-130">[AuthorizeAreaPage přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="bbca8-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="bbca8-131">Vyžaduje autorizaci pro přístup ke složce oblastí</span><span class="sxs-lookup"><span data-stu-id="bbca8-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="bbca8-132">Použití [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) pro všechny oblasti ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="bbca8-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="bbca8-133">Cesta ke složce je cesta ke složce relativní k stránky kořenový adresář pro zadanou oblast.</span><span class="sxs-lookup"><span data-stu-id="bbca8-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="bbca8-134">Například cesta ke složce pro soubory v rámci *oblasti/Identity/stránek/Správa nebo* je *a správa*.</span><span class="sxs-lookup"><span data-stu-id="bbca8-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="bbca8-135">[AuthorizeAreaFolder přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="bbca8-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="bbca8-136">Povolit anonymní přístup na stránku</span><span class="sxs-lookup"><span data-stu-id="bbca8-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="bbca8-137">Použití [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na stránku v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="bbca8-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="bbca8-138">Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor bez rozšíření a který obsahuje pouze lomítka.</span><span class="sxs-lookup"><span data-stu-id="bbca8-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="bbca8-139">Povolit anonymní přístup do složky stránek</span><span class="sxs-lookup"><span data-stu-id="bbca8-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="bbca8-140">Použití [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na všechny stránky ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="bbca8-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="bbca8-141">Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="bbca8-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="bbca8-142">Poznámky ke kombinování oprávnění a anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="bbca8-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="bbca8-143">Je naprosto platný určíte, že složka stránek vyžadují autorizace a určit, že stránku v rámci dané složky umožňuje anonymní přístup:</span><span class="sxs-lookup"><span data-stu-id="bbca8-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="bbca8-144">Naopak, není ale hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="bbca8-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="bbca8-145">Nelze deklarovat složku stránek pro anonymní přístup a určení stránky v rámci pro autorizaci:</span><span class="sxs-lookup"><span data-stu-id="bbca8-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="bbca8-146">Vyžadování ověření na stránce privátní nebude fungovat, protože při i `AllowAnonymousFilter` a `AuthorizeFilter` filtry se použijí na stránku `AllowAnonymousFilter` wins a řídí přístup.</span><span class="sxs-lookup"><span data-stu-id="bbca8-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bbca8-147">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="bbca8-147">Additional resources</span></span>

* [<span data-ttu-id="bbca8-148">Razor stránek vlastní trasy a stránky model poskytovatele</span><span class="sxs-lookup"><span data-stu-id="bbca8-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="bbca8-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) třídy</span><span class="sxs-lookup"><span data-stu-id="bbca8-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
