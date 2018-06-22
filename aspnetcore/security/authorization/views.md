---
title: Ověření na základě zobrazení v rozhraní ASP.NET MVC jádra
author: rick-anderson
description: Tento dokument ukazuje, jak využívat službu autorizace uvnitř zobrazení syntaxe Razor rozhraní ASP.NET Core a vložit.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: f25bab61afc93ff14bfd9c36d95a6d2e54b06dfb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277807"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Ověření na základě zobrazení v rozhraní ASP.NET MVC jádra

Vývojář často chce zobrazení, skrytí nebo v opačném případě upravte uživatelského rozhraní na základě aktuální identity uživatele. Můžete přístup ke službě ověřování v rámci zobrazení MVC prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Chcete-li vložit služby autorizace do zobrazení Razor, použijte `@inject` – direktiva:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Pokud chcete službu ověřování v každé zobrazení, umístit `@inject` direktivy do *_ViewImports.cshtml* soubor *zobrazení* adresáře. Další informace najdete v tématu [vkládání závislostí do zobrazení](xref:mvc/views/dependency-injection).

Používat službu vloženého autorizace k vyvolání `AuthorizeAsync` v úplně stejně, jako by kontrolovat při [autorizace na základě prostředků](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

V některých případech bude prostředek zobrazení modelu. Vyvolání `AuthorizeAsync` v úplně stejně, jako by kontrolovat při [autorizace na základě prostředků](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

V předchozí kód model předán jako prostředek, kterou má provést vyhodnocení zásad v úvahu.

> [!WARNING]
> Nemusíte spoléhat na přepnutím viditelnost prvků uživatelského rozhraní aplikace jako jedinou autorizace kontroly. Skrytí elementu uživatelského rozhraní nemusí úplně zamezit přístupu k jeho přidruženému kontroleru akce. Představte si třeba tlačítko v předchozím fragmentu kódu. Uživatele můžete vyvolat `Edit` metody akce, pokud uživatel zná prostředků relativní adresa URL je */Document/Edit/1*. Z tohoto důvodu `Edit` metoda akce proveďte vlastní kontroly autorizace.
