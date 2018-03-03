---
title: "Konfigurace Identity primární klíče datový typ"
author: AdrienTorris
description: "Tento článek popisuje kroky pro konfiguraci požadovaný datový typ používaný pro ASP.NET Core Identity primární klíč."
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: ff1c3aff3ea833081a25ea5fc4f2c2b65823f536
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>Konfigurace ASP.NET Core Identity primární klíče datový typ

Jádro ASP.NET Identity můžete konfigurovat datový typ, který používá k reprezentování primární klíč. Používá identity `string` datový typ ve výchozím nastavení. Toto chování můžete přepsat.

## <a name="customize-the-primary-key-data-type"></a>Přizpůsobení primární klíče datový typ

1. Vytvořit vlastní implementaci [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) třídy. Představuje typ, který má být použit pro vytvoření uživatelské objekty. V následujícím příkladu, výchozí `string` se nahradí typ `Guid`.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. Vytvořit vlastní implementaci [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) třídy. Představuje typ, který se má použít pro vytváření objektů role. V následujícím příkladu, výchozí `string` se nahradí typ `Guid`.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. Vytvoření třídy kontextu vlastní databázi. Dědí z třídy kontextu databáze Entity Framework použitý pro identitu. `TUser` a `TRole` argumenty odkazovat na vlastní třídy pro uživatele a role vytvořili v předchozím kroku, v uvedeném pořadí. `Guid` Datový typ je definována pro primární klíč.

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. Registrace třídy kontextu vlastní databázi při přidávání Identity služby v třída při spuštění aplikace.

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)
    
    `AddEntityFrameworkStores` Metoda nebude přijímat `TKey` argument, protože nebyla v ASP.NET Core 1.x. Datový typ primární klíč je odvodit analýzou `DbContext` objektu.
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)
    
    `AddEntityFrameworkStores` Metoda přijímá `TKey` argument označující typ dat primární klíč.
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>Testování změny

Po dokončení změny konfigurace odráží vlastnost představující primární klíč nového datového typu. Následující příklad ukazuje, přístupu k vlastnosti v kontroler MVC.

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
