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
ms.openlocfilehash: 66631e46640e294c934aa563518509b96f5cd158
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="eaea4-103">Konfigurace ASP.NET Core Identity primární klíče datový typ</span><span class="sxs-lookup"><span data-stu-id="eaea4-103">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="eaea4-104">Jádro ASP.NET Identity můžete konfigurovat datový typ, který používá k reprezentování primární klíč.</span><span class="sxs-lookup"><span data-stu-id="eaea4-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="eaea4-105">Používá identity `string` datový typ ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="eaea4-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="eaea4-106">Toto chování můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="eaea4-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="eaea4-107">Přizpůsobení primární klíče datový typ</span><span class="sxs-lookup"><span data-stu-id="eaea4-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="eaea4-108">Vytvořit vlastní implementaci [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) třídy.</span><span class="sxs-lookup"><span data-stu-id="eaea4-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="eaea4-109">Představuje typ, který má být použit pro vytvoření uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="eaea4-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="eaea4-110">V následujícím příkladu, výchozí `string` se nahradí typ `Guid`.</span><span class="sxs-lookup"><span data-stu-id="eaea4-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="eaea4-111">Vytvořit vlastní implementaci [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) třídy.</span><span class="sxs-lookup"><span data-stu-id="eaea4-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="eaea4-112">Představuje typ, který se má použít pro vytváření objektů role.</span><span class="sxs-lookup"><span data-stu-id="eaea4-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="eaea4-113">V následujícím příkladu, výchozí `string` se nahradí typ `Guid`.</span><span class="sxs-lookup"><span data-stu-id="eaea4-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="eaea4-114">Vytvoření třídy kontextu vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="eaea4-114">Create a custom database context class.</span></span> <span data-ttu-id="eaea4-115">Dědí z třídy kontextu databáze Entity Framework použitý pro identitu.</span><span class="sxs-lookup"><span data-stu-id="eaea4-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="eaea4-116">`TUser` a `TRole` argumenty odkazovat na vlastní třídy pro uživatele a role vytvořili v předchozím kroku, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="eaea4-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="eaea4-117">`Guid` Datový typ je definována pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="eaea4-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="eaea4-118">Registrace třídy kontextu vlastní databázi při přidávání Identity služby v třída při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="eaea4-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="eaea4-119">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="eaea4-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="eaea4-120">`AddEntityFrameworkStores` Metoda nebude přijímat `TKey` argument, protože nebyla v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="eaea4-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="eaea4-121">Datový typ primární klíč je odvodit analýzou `DbContext` objektu.</span><span class="sxs-lookup"><span data-stu-id="eaea4-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="eaea4-122">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="eaea4-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="eaea4-123">`AddEntityFrameworkStores` Metoda přijímá `TKey` argument označující typ dat primární klíč.</span><span class="sxs-lookup"><span data-stu-id="eaea4-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="eaea4-124">Testování změny</span><span class="sxs-lookup"><span data-stu-id="eaea4-124">Test the changes</span></span>

<span data-ttu-id="eaea4-125">Po dokončení změny konfigurace odráží vlastnost představující primární klíč nového datového typu.</span><span class="sxs-lookup"><span data-stu-id="eaea4-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="eaea4-126">Následující příklad ukazuje, přístupu k vlastnosti v kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="eaea4-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
