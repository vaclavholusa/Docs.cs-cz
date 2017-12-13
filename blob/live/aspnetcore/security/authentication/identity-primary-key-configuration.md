---
title: "Konfigurace Identity primární klíče datový typ"
author: AdrienTorris
description: "Tento článek popisuje kroky pro konfiguraci požadovaný datový typ používaný pro ASP.NET Core Identity primární klíč."
keywords: "ASP.NET Core, Identity, primární klíč"
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 5734a9aa86fb2831bd054593ad41c3e3bda4729e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="e4eda-104">Konfigurace ASP.NET Core Identity primární klíče datový typ</span><span class="sxs-lookup"><span data-stu-id="e4eda-104">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="e4eda-105">Jádro ASP.NET Identity můžete konfigurovat datový typ, který používá k reprezentování primární klíč.</span><span class="sxs-lookup"><span data-stu-id="e4eda-105">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="e4eda-106">Používá identity `string` datový typ ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="e4eda-106">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="e4eda-107">Toto chování můžete přepsat.</span><span class="sxs-lookup"><span data-stu-id="e4eda-107">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="e4eda-108">Přizpůsobení primární klíče datový typ</span><span class="sxs-lookup"><span data-stu-id="e4eda-108">Customize the primary key data type</span></span>

1. <span data-ttu-id="e4eda-109">Vytvořit vlastní implementaci [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) třídy.</span><span class="sxs-lookup"><span data-stu-id="e4eda-109">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="e4eda-110">Představuje typ, který má být použit pro vytvoření uživatelské objekty.</span><span class="sxs-lookup"><span data-stu-id="e4eda-110">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="e4eda-111">V následujícím příkladu, výchozí `string` se nahradí typ `Guid`.</span><span class="sxs-lookup"><span data-stu-id="e4eda-111">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="e4eda-112">Vytvořit vlastní implementaci [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) třídy.</span><span class="sxs-lookup"><span data-stu-id="e4eda-112">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="e4eda-113">Představuje typ, který se má použít pro vytváření objektů role.</span><span class="sxs-lookup"><span data-stu-id="e4eda-113">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="e4eda-114">V následujícím příkladu, výchozí `string` se nahradí typ `Guid`.</span><span class="sxs-lookup"><span data-stu-id="e4eda-114">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="e4eda-115">Vytvoření třídy kontextu vlastní databázi.</span><span class="sxs-lookup"><span data-stu-id="e4eda-115">Create a custom database context class.</span></span> <span data-ttu-id="e4eda-116">Dědí z třídy kontextu databáze Entity Framework použitý pro identitu.</span><span class="sxs-lookup"><span data-stu-id="e4eda-116">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="e4eda-117">`TUser` a `TRole` argumenty odkazovat na vlastní třídy pro uživatele a role vytvořili v předchozím kroku, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="e4eda-117">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="e4eda-118">`Guid` Datový typ je definována pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="e4eda-118">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="e4eda-119">Registrace třídy kontextu vlastní databázi při přidávání Identity služby v třída při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="e4eda-119">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e4eda-120">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="e4eda-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="e4eda-121">`AddEntityFrameworkStores` Metoda nebude přijímat `TKey` argument, protože nebyla v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e4eda-121">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="e4eda-122">Datový typ primární klíč je odvodit analýzou `DbContext` objektu.</span><span class="sxs-lookup"><span data-stu-id="e4eda-122">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e4eda-123">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="e4eda-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="e4eda-124">`AddEntityFrameworkStores` Metoda přijímá `TKey` argument označující typ dat primární klíč.</span><span class="sxs-lookup"><span data-stu-id="e4eda-124">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="e4eda-125">Testování změny</span><span class="sxs-lookup"><span data-stu-id="e4eda-125">Test the changes</span></span>

<span data-ttu-id="e4eda-126">Po dokončení změny konfigurace odráží vlastnost představující primární klíč nového datového typu.</span><span class="sxs-lookup"><span data-stu-id="e4eda-126">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="e4eda-127">Následující příklad ukazuje, přístupu k vlastnosti v kontroler MVC.</span><span class="sxs-lookup"><span data-stu-id="e4eda-127">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
