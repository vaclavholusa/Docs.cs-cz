---
title: "Konvence autorizace stránky Razor v ASP.NET Core"
author: guardrex
description: "Zjistěte, jak řídit přístup k stránky s konvence při spuštění, které autorizace uživatelů a anonymní uživatelům umožní přístup k jednotlivým stránkám nebo složky stránek."
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.assetid: f65ad22d-9472-478a-856c-c59c8681fa71
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 36acf3c06a462882972c5f389d544d98cadc35f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="47817-103">Konvence autorizace stránky Razor v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47817-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="47817-104">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="47817-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="47817-105">Jedním ze způsobů k řízení přístupu ve vaší aplikaci stránky Razor je použití autorizace konvence při spuštění.</span><span class="sxs-lookup"><span data-stu-id="47817-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="47817-106">Tyto konvence umožňují autorizace uživatelů a anonymní uživatelům umožní přístup k jednotlivým stránkám nebo složky stránek.</span><span class="sxs-lookup"><span data-stu-id="47817-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="47817-107">Platí zásady popsané v tomto tématu automaticky [filtry autorizace](xref:mvc/controllers/filters#authorization-filters) pro řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="47817-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="47817-108">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47817-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="47817-109">Vyžadovat oprávnění pro přístup k stránky</span><span class="sxs-lookup"><span data-stu-id="47817-109">Require authorization to access a page</span></span>

<span data-ttu-id="47817-110">Použití [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stránku v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="47817-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="47817-111">Zadaná cesta je cesta modul zobrazení, která je relativní cesta stránky Razor kořenové bez rozšíření a obsahující jenom lomítka.</span><span class="sxs-lookup"><span data-stu-id="47817-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="47817-112">[AuthorizePage přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="47817-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="47817-113">Vyžadovat autorizace pro přístup ke složce stránek</span><span class="sxs-lookup"><span data-stu-id="47817-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="47817-114">Použití [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na všechny stránky ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="47817-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="47817-115">Zadaná cesta je cesta modul zobrazení, která je relativní cesta kořenové stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="47817-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="47817-116">[AuthorizeFolder přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.</span><span class="sxs-lookup"><span data-stu-id="47817-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="47817-117">Povolení anonymního přístupu na stránku</span><span class="sxs-lookup"><span data-stu-id="47817-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="47817-118">Použití [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na stránku v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="47817-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="47817-119">Zadaná cesta je cesta modul zobrazení, která je relativní cesta stránky Razor kořenové bez rozšíření a obsahující jenom lomítka.</span><span class="sxs-lookup"><span data-stu-id="47817-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="47817-120">Povolit anonymní přístup do složky stránek</span><span class="sxs-lookup"><span data-stu-id="47817-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="47817-121">Použití [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na všechny stránky ve složce v zadané cestě:</span><span class="sxs-lookup"><span data-stu-id="47817-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="47817-122">Zadaná cesta je cesta modul zobrazení, která je relativní cesta kořenové stránky Razor.</span><span class="sxs-lookup"><span data-stu-id="47817-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="47817-123">Poznámka: na kombinování oprávnění a anonymní přístup</span><span class="sxs-lookup"><span data-stu-id="47817-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="47817-124">Je zcela vhodný k určení, že složku stránek vyžadují autorizace a určit, že na stránce v této složce umožňuje anonymní přístup:</span><span class="sxs-lookup"><span data-stu-id="47817-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="47817-125">Naopak, ale není pravda.</span><span class="sxs-lookup"><span data-stu-id="47817-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="47817-126">Nelze deklarovat složku stránek pro anonymní přístup a určení stránky v rámci pro ověřování:</span><span class="sxs-lookup"><span data-stu-id="47817-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="47817-127">Vyžadující ověření na stránce privátní nebude fungovat, protože při jak `AllowAnonymousFilter` a `AuthorizeFilter` na stránce jsou použity filtry `AllowAnonymousFilter` wins a řídí přístup.</span><span class="sxs-lookup"><span data-stu-id="47817-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="47817-128">Viz také</span><span class="sxs-lookup"><span data-stu-id="47817-128">See also</span></span>

* [<span data-ttu-id="47817-129">Syntaxe Razor stránky vlastní trasy a stránka zprostředkovatele modelu</span><span class="sxs-lookup"><span data-stu-id="47817-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="47817-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) – třída</span><span class="sxs-lookup"><span data-stu-id="47817-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
