---
title: "Vkládání závislostí v obslužné rutiny požadavku"
author: rick-anderson
description: "Tento dokument popisuje postup vložit obslužné rutiny požadavek ověřování do aplikace ASP.NET Core pomocí vkládání závislostí."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 1b7506b49109264a8c628ea2e39ded9f5ace95d3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a>Vkládání závislostí v obslužné rutiny požadavku

<a name="security-authorization-di"></a>

[Autorizace obslužné rutiny musí být zaregistrovaný](policies.md#handler-registration) v kolekci služby během konfigurace (pomocí [vkládání závislostí](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

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
