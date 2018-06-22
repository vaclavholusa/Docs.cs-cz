---
title: Vkládání závislostí v obslužné rutiny požadavku v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak se vložit obslužné rutiny požadavek ověřování do aplikace ASP.NET Core pomocí vkládání závislostí.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273719"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Vkládání závislostí v obslužné rutiny požadavku v ASP.NET Core

<a name="security-authorization-di"></a>

[Autorizace obslužné rutiny musí být zaregistrovaný](xref:security/authorization/policies#handler-registration) v kolekci služby během konfigurace (pomocí [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

Předpokládejme, že jste měli úložiště pravidel, kterou chcete vyhodnotit uvnitř obslužné rutiny autorizace a že úložiště se zaregistroval v kolekci služby. Autorizace se vyřešit a který vložit do vaší konstruktor.

Například pokud jste chtěli použít ASP. NET je protokolování infrastrukturu, kterou chcete vložit `ILoggerFactory` do vaší obslužné rutiny. Tato obslužná rutina může vypadat například:

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

Zaregistrujete by obslužná rutina s `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Instance obslužné rutiny bude vytvořen při spuštění aplikace a bude DI vložit zaregistrovanou `ILoggerFactory` do vaší konstruktor.

> [!NOTE]
> Obslužné rutiny, které používají rozhraní Entity Framework by neměl být registrován jako jednotlivé prvky.
