---
title: Vytváření názvů autorizace stránek Razor v ASP.NET Core
author: guardrex
description: Zjistěte, jak řídit přístup ke stránkám s vytváření názvů, které uživateli uživatele autorizovat a umožňoval anonymním uživatelům přístup k stránky nebo složky stránek.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207261"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Vytváření názvů autorizace stránek Razor v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Jedním ze způsobů řízení přístupu v aplikaci pro stránky Razor je použití autorizace konvence při spuštění. Tato konvence umožňují uživateli uživatele autorizovat a umožňoval anonymním uživatelům přístup k jednotlivé stránky nebo složky stránek. Platí zásady popsané v tomto tématu automaticky [filtry autorizace](xref:mvc/controllers/filters#authorization-filters) pro řízení přístupu.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([stažení](xref:index#how-to-download-a-sample))

Tato ukázková aplikace používá [ověřování souborem Cookie bez ASP.NET Core Identity](xref:security/authentication/cookie). Uživatelský účet pro hypotetické uživatele Marie Rodriguez, je pevně zakódované do aplikace. Použití uživatelského jména e-mailu "maria.rodriguez@contoso.com" a jakékoli heslo k přihlášení uživatele. Ověření uživatele v `AuthenticateUser` metodu *Pages/Account/Login.cshtml.cs* souboru. V reálný příklad uživatel by ověřovány proti databázi. ASP.NET Core Identity, postupujte podle pokynů v [Úvod do Identity v ASP.NET Core](xref:security/authentication/identity) tématu. Koncepty a příkladů uvedených v tomto tématu platí stejně pro aplikace, které používají technologii ASP.NET Core Identity.

## <a name="require-authorization-to-access-a-page"></a>Vyžaduje povolení ke stránce

Použití [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na stránku v zadané cestě:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor bez rozšíření a který obsahuje pouze lomítka.

[AuthorizePage přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> `AuthorizeFilter` Lze použít u třídy modelu stránky s `[Authorize]` atribut filtru. Další informace najdete v tématu [atribut filtru Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Vyžaduje autorizaci pro přístup ke složce stránek

Použití [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) na všechny stránky ve složce v zadané cestě:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor.

[AuthorizeFolder přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Vyžaduje autorizaci pro přístup k stránku oblasti

Použití [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) stránku oblasti v zadané cestě:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Název stránky je cesta k souboru bez přípony vzhledem k stránky kořenový adresář pro zadanou oblast. Například název stránky pro soubor *Areas/Identity/Pages/Manage/Accounts.cshtml* je */Správa nebo účty*.

[AuthorizeAreaPage přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Vyžaduje autorizaci pro přístup ke složce oblastí

Použití [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) pro všechny oblasti ve složce v zadané cestě:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Cesta ke složce je cesta ke složce relativní k stránky kořenový adresář pro zadanou oblast. Například cesta ke složce pro soubory v rámci *oblasti/Identity/stránek/Správa nebo* je *a správa*.

[AuthorizeAreaFolder přetížení](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) je k dispozici, pokud je třeba zadat zásad autorizace.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Povolit anonymní přístup na stránku

Použití [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na stránku v zadané cestě:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor bez rozšíření a který obsahuje pouze lomítka.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Povolit anonymní přístup do složky stránek

Použití [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) konvence prostřednictvím [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) přidat [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) na všechny stránky ve složce v zadané cestě:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Zadaná cesta je cesta modulu zobrazení, která je relativní cesta kořenového stránky Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Poznámky ke kombinování oprávnění a anonymní přístup

Je naprosto platný určíte, že složka stránek vyžadují autorizace a určit, že stránku v rámci dané složky umožňuje anonymní přístup:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Naopak, není ale hodnotu true. Nelze deklarovat složku stránek pro anonymní přístup a určení stránky v rámci pro autorizaci:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Vyžadování ověření na stránce privátní nebude fungovat, protože při i `AllowAnonymousFilter` a `AuthorizeFilter` filtry se použijí na stránku `AllowAnonymousFilter` wins a řídí přístup.

## <a name="additional-resources"></a>Další zdroje

* [Razor stránek vlastní trasy a stránky model poskytovatele](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) třídy
