---
title: "Částí aplikace v ASP.NET Core"
author: ardalis
description: "Další informace o použití částí aplikace, které jsou abstrations přes prostředky aplikace, ke konfiguraci vaší aplikace na zjišťování nebo předejít přetížení funkce ze sestavení."
keywords: "ASP.NET Core, část aplikace, součást aplikace"
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a260675e7461105d4f6a0c61fd13971663c268f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="application-parts-in-aspnet-core"></a>Částí aplikace v ASP.NET Core

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

*Část aplikace* je abstrakci přes prostředky aplikace, ze kterého MVC funkce jako řadiče zobrazení součásti, nebo může být zjištěny značky pomocné rutiny. Příkladem je součástí aplikace je AssemblyPart, který zapouzdřuje odkaz na sestavení a zpřístupňuje typy a odkazy na kompilace. *Funkce zprostředkovatelé* práce s částí aplikace k naplnění funkce aplikace ASP.NET MVC jádra. Nejčastěji se využívá případ částí aplikace je vám umožní nakonfigurovat aplikace zjistit (nebo vyloučit načítání) MVC funkce ze sestavení.

## <a name="introducing-application-parts"></a>Představení částí aplikace

Aplikace MVC načíst jejich funkce z [částí aplikace](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). Konkrétně [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) třída reprezentuje z části aplikace, kterou je zajištěna sestavení. Tyto třídy můžete použít ke zjištění a načíst MVC funkcí, jako jsou řadiče, zobrazení součásti, pomocné rutiny značky a razor kompilace zdrojů. [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) zodpovídá za sledování částí aplikace a poskytovatelů funkcí, které jsou k dispozici v aplikaci MVC. Můžete pracovat s `ApplicationPartManager` v `Startup` při konfiguraci MVC:

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

Ve výchozím nastavení se MVC vyhledávání strom závislosti a vyhledat řadiče (i v jiných sestavení). Načíst libovolný sestavení (například z modulu plug-in, který se odkazuje v době kompilace), můžete použít jako součást aplikace.

Můžete použít částí aplikace k *vyhnout* hledá řadičů v konkrétní sestavení nebo umístění. Můžete řídit, které části (nebo sestavení) jsou k dispozici pro aplikaci změnou `ApplicationParts` kolekce `ApplicationPartManager`. Pořadí položek v `ApplicationParts` kolekce není důležité. Je důležité plně nakonfigurovat `ApplicationPartManager` před jeho použitím v kontejneru konfigurace služby. Například byste měli plně nakonfigurovat `ApplicationPartManager` před vyvoláním `AddControllersAsServices`. Selhání Uděláte to tak, bude znamenat, že řadiče v částí aplikace přidán po, nebude mít vliv volání metody (nebude získat registrován jako služby) může být důsledkem toho nesprávné bevavior vaší aplikace.

Pokud je sestavení obsahující řadiče nechcete používat, odeberte jej z `ApplicationPartManager`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

Kromě sestavení projektu a jeho závislá sestavení `ApplicationPartManager` bude obsahovat části pro `Microsoft.AspNetCore.Mvc.TagHelpers` a `Microsoft.AspNetCore.Mvc.Razor` ve výchozím nastavení.

## <a name="application-feature-providers"></a>Zprostředkovatele funkce aplikace

Zprostředkovatele funkce aplikace zkontrolujte částí aplikace a poskytují funkce pro ty části. Existuje poskytovatelů integrované funkce pro následující funkce MVC:

* [Řadiče](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Odkaz na metadata](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Pomocníci značky](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Zobrazení součásti](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Zprostředkovatelé funkce dědit z `IApplicationFeatureProvider<T>`, kde `T` je typ funkce. Můžete implementovat vlastní funkce, kterou zprostředkovatele pro jakýkoli z typů funkce MVC uvedené výše. Pořadí poskytovatelů funkce v `ApplicationPartManager.FeatureProviders` kolekce může být důležité, protože novější zprostředkovatelé může reagovat na akce provedené předchozí poskytovatelů.

### <a name="sample-generic-controller-feature"></a>Ukázka: Funkce obecné adaptér

Ve výchozím nastavení, ASP.NET MVC základní ignoruje obecné řadiče (například `SomeController<T>`). Tato ukázka používá zprostředkovatele funkce řadiče, který spustí po výchozím zprostředkovatelem a přidá instance obecné řadiče pro zadaný seznam typů (definované v `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Typy entit:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Zprostředkovatele funkce je přidaný do `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Ve výchozím nastavení, Obecný řadič názvy používaných pro směrování by být ve formátu *GenericController 1 [pomůcky]* místo *pomůcky*. Upravit název tak, aby odpovídaly obecného typu používané řadičem se používá následující atribut:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

`GenericController` Třídy:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Výsledek, pokud se požaduje odpovídající trasy:

![Příklad výstup z ukázkové aplikace načte "Hello z obecné Sproket řadiče."](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Ukázka: Zobrazení dostupných funkcí

Můžete iterovat vyplněná funkce dostupné pro vaši aplikaci tím, že požádá `ApplicationPartManager` prostřednictvím [vkládání závislostí](../../fundamentals/dependency-injection.md) a jeho použití k naplnění instancí příslušné funkce:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Příklad výstupu:

![Příklad výstupu z ukázkové aplikace](app-parts/_static/available-features.png)
