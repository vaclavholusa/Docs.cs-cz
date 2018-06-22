---
title: Konvence autorizace stránky Razor v ASP.NET Core
author: guardrex
description: Zjistěte, jak řídit přístup k stránky s názvů autorizace uživatelů a anonymní uživatelům umožní přístup k stránky nebo složky stránek.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272672"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="a6d2e-103">Konvence autorizace stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6d2e-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="a6d2e-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a6d2e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a6d2e-105">Jedním ze způsobů k řízení přístupu ve vaší aplikaci stránky Razor je použití autorizace konvence při spuštění.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="a6d2e-106">Tyto konvence umožňují autorizace uživatelů a anonymní uživatelům umožní přístup k jednotlivým stránkám nebo složky stránek.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="a6d2e-107">Platí zásady popsané v tomto tématu automaticky [filtry autorizace](xref:mvc/controllers/filters#authorization-filters) pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="a6d2e-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a6d2e-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a6d2e-109">Použití ukázkové aplikace [ověřování souborů Cookie bez ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="a6d2e-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="a6d2e-110">Uživatelský účet pro hypotetické uživatele Marie Rodriguez je pevně zakódované do aplikace.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="a6d2e-111">Použití uživatelského jména e-mailu "maria.rodriguez@contoso.com" a žádné heslo pro přihlášení uživatele.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="a6d2e-112">Ověření uživatele v `AuthenticateUser` metoda v *Pages/Account/Login.cshtml.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="a6d2e-113">V příkladu reálného uživatele by ověřovány proti databázi.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="a6d2e-114">ASP.NET Core Identity, postupujte podle pokynů v [Úvod do identita na ASP.NET Core](xref:security/authentication/identity) tématu.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="a6d2e-115">Koncepty a příklady uvedené v tomto tématu platí stejně pro aplikace, které používají ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="a6d2e-116">Vyžadovat oprávnění pro přístup k stránky</span><span class="sxs-lookup"><span data-stu-id="a6d2e-116">Require authorization to access a page</span></span>

<span data-ttu-id="a6d2e-117">Použití [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stránku v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="a6d2e-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="a6d2e-118">Zadaná cesta je cesta modul zobrazení, která je relativní cesta stránky Razor kořenové bez rozšíření a obsahující jenom lomítka.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="a6d2e-119">[AuthorizePage přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="a6d2e-120">`AuthorizeFilter` Lze použít pro třídu modelu stránky s `[Authorize]` atribut filtru.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="a6d2e-121">Další informace najdete v tématu [atribut filtru Autorizovat](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="a6d2e-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="a6d2e-122">Vyžadovat autorizace pro přístup ke složce stránek</span><span class="sxs-lookup"><span data-stu-id="a6d2e-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="a6d2e-123">Použití [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na všechny stránky ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="a6d2e-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="a6d2e-124">Zadaná cesta je cesta modul zobrazení, která je relativní cesta kořenové stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="a6d2e-125">[AuthorizeFolder přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="a6d2e-126">Povolení anonymního přístupu na stránku</span><span class="sxs-lookup"><span data-stu-id="a6d2e-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="a6d2e-127">Použití [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na stránku v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="a6d2e-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="a6d2e-128">Zadaná cesta je cesta modul zobrazení, která je relativní cesta stránky Razor kořenové bez rozšíření a obsahující jenom lomítka.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="a6d2e-129">Povolit anonymní přístup do složky stránek</span><span class="sxs-lookup"><span data-stu-id="a6d2e-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="a6d2e-130">Použití [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na všechny stránky ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="a6d2e-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="a6d2e-131">Zadaná cesta je cesta modul zobrazení, která je relativní cesta kořenové stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="a6d2e-132">Poznámka: na kombinování oprávnění a anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="a6d2e-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="a6d2e-133">Je zcela vhodný k určení, že složku stránek vyžadují autorizace a určit, že na stránce v této složce umožňuje anonymní přístup:</span><span class="sxs-lookup"><span data-stu-id="a6d2e-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="a6d2e-134">Naopak, ale není pravda.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="a6d2e-135">Nelze deklarovat složku stránek pro anonymní přístup a určení stránky v rámci pro ověřování:</span><span class="sxs-lookup"><span data-stu-id="a6d2e-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="a6d2e-136">Vyžadující ověření na stránce privátní nebude fungovat, protože při jak `AllowAnonymousFilter` a `AuthorizeFilter` na stránce jsou použity filtry `AllowAnonymousFilter` wins a řídí přístup.</span><span class="sxs-lookup"><span data-stu-id="a6d2e-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6d2e-137">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="a6d2e-137">Additional resources</span></span>

* [<span data-ttu-id="a6d2e-138">Syntaxe Razor stránky vlastní trasy a stránka zprostředkovatele modelu</span><span class="sxs-lookup"><span data-stu-id="a6d2e-138">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="a6d2e-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) – třída</span><span class="sxs-lookup"><span data-stu-id="a6d2e-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
