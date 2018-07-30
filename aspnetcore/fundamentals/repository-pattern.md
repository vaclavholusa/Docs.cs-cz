---
title: Použitému vzoru úložišť s ASP.NET Core
author: ardalis
description: Zjistěte, jak implementovat vzor návrhu úložiště aplikace v aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342740"
---
# <a name="repository-pattern-with-aspnet-core"></a>Použitému vzoru úložišť s ASP.NET Core

Podle [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), a [Luke Latham](https://github.com/guardrex)

*Použitému vzoru úložišť* je návrhový vzor, který izoluje přístup k datům za abstrakce rozhraní. Připojení k databázi a manipulace s objekty úložiště dat se provádí prostřednictvím metody poskytované objektem implementace rozhraní. V důsledku toho není nutné pro volání kódu k řešení databázi aspekty, jako je připojení, příkazy a čtenáři.

Implementace vzoru úložiště pomocí ASP.NET Core má následující výhody:

* Organizace aplikace není tak složitý s žádné přímé závislost mezi firmy a vrstvy přístupu k datům.
* Je snazší znovu použít kód pro přístup k databázi, protože kód je centrálně spravované pomocí jednoho nebo více úložišť.
* Obchodní domény může být nezávisle na sobě jednotky testování z databázové vrstvě.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

[Ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) používá vzor úložiště k inicializaci a zobrazit seznam názvů film znak. Tato aplikace používá [Entity Framework Core](/ef/core/) a `ApplicationDbContext` třídy pro jeho trvalosti dat, ale infrastruktury databáze není zřejmé tam, kde je k datům přistupuje. Data access a databázových objektů jsou abstrahované za [úložiště](https://martinfowler.com/eaaCatalog/repository.html).

## <a name="repository-interface"></a>Rozhraní úložiště

Úložiště rozhraní definuje vlastnosti a metody pro implementaci. V ukázkové aplikaci rozhraní úložiště pro data o filmech znak je `ICharacterRepository`. `ICharacterRepository` definuje `ListAll` a `Add` metod požadovaných pro práci s `Character` instance v aplikaci:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

`Character` je definována takto:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a>Konkrétní typ úložiště

Rozhraní je implementováno konkrétního typu implementujícího typ. V ukázkové aplikaci `CharacterRepository` spravuje kontext databáze a implementuje `ListAll` a `Add` metody:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a>Registrace služby úložiště

Místní úložiště a databáze jsou zaregistrované v kontejneru služby `Startup.ConfigureServices`. V ukázkové aplikaci `ApplicationDbContext` je nakonfigurovaný pomocí volání metody rozšíření [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext). `ICharacterRepository` je zaregistrovaný jako vymezené služby:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a>Vloží instanci úložiště

Ve třídě, ve kterém jsou vyžadována přístup k databázi je instance úložiště požadované prostřednictvím konstruktoru a přiřadit soukromé pole pro použití metody třídy. V ukázkové aplikaci `ICharacterRepository` se používá ke:

* Naplnění databáze, pokud neexistuje žádné znaky.
* Získání seznamu sad znaků pro zobrazení.

Všimněte si, jak volající kód pouze komunikuje s implementací rozhraní `CharacterRepository`. Volání kódu nepoužívá `ApplicationDbContext` přímo:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a>Rozhraní obecného úložiště

Toto téma a jeho ukázkovou aplikaci ukazují nejjednodušší implementace tohoto modelu úložiště, kde se vytvoří jedno úložiště pro každý objekt firmy. Pokud aplikace překročí několik objektů *obecného úložiště rozhraní* může snížit množství kódu vyžadovaného pro implementaci vzoru úložiště. Další informace najdete v tématu [DevIQ: použitému vzoru úložišť: Obecné rozhraní úložiště](http://deviq.com/repository-pattern/).

## <a name="additional-resources"></a>Další zdroje

* [DevIQ: Použitému vzoru úložišť](https://deviq.com/repository-pattern/)
* [Injektáž závislostí](xref:fundamentals/dependency-injection)
* [Injektáž závislostí do zobrazení](xref:mvc/views/dependency-injection)
* [Injektáž závislostí do kontrolerů](xref:mvc/controllers/dependency-injection)
* [Injektáž závislostí v obslužných rutinách požadavků](xref:security/authorization/dependencyinjection)
* [Inverze – kontejnery ovládacích prvků a vzor injektáž závislostí](https://www.martinfowler.com/articles/injection.html)
