---
title: "Vkládání závislostí v obslužné rutiny požadavku"
author: rick-anderson
description: "Tento dokument popisuje postup vložit obslužné rutiny požadavek ověřování do aplikace ASP.NET Core pomocí vkládání závislostí."
keywords: "ASP.NET Core, vkládání závislostí, obslužná rutina ověřování"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
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
