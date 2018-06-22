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
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="8d7fa-103">Vkládání závislostí v obslužné rutiny požadavku v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8d7fa-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="8d7fa-104">[Autorizace obslužné rutiny musí být zaregistrovaný](xref:security/authorization/policies#handler-registration) v kolekci služby během konfigurace (pomocí [vkládání závislostí](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="8d7fa-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="8d7fa-105">Předpokládejme, že jste měli úložiště pravidel, kterou chcete vyhodnotit uvnitř obslužné rutiny autorizace a že úložiště se zaregistroval v kolekci služby.</span><span class="sxs-lookup"><span data-stu-id="8d7fa-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="8d7fa-106">Autorizace se vyřešit a který vložit do vaší konstruktor.</span><span class="sxs-lookup"><span data-stu-id="8d7fa-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="8d7fa-107">Například pokud jste chtěli použít ASP. NET je protokolování infrastrukturu, kterou chcete vložit `ILoggerFactory` do vaší obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="8d7fa-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="8d7fa-108">Tato obslužná rutina může vypadat například:</span><span class="sxs-lookup"><span data-stu-id="8d7fa-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="8d7fa-109">Zaregistrujete by obslužná rutina s `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="8d7fa-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="8d7fa-110">Instance obslužné rutiny bude vytvořen při spuštění aplikace a bude DI vložit zaregistrovanou `ILoggerFactory` do vaší konstruktor.</span><span class="sxs-lookup"><span data-stu-id="8d7fa-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="8d7fa-111">Obslužné rutiny, které používají rozhraní Entity Framework by neměl být registrován jako jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="8d7fa-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
