---
title: "Vkládání závislostí do řadiče"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: d8253858864efa85f0d2a2175669dc27b879b175
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="dependency-injection-into-controllers"></a>Vkládání závislostí do řadiče

<a name="dependency-injection-controllers"></a>

Podle [Steve Smith](https://ardalis.com/)

Kontrolery MVC ASP.NET Core měli požádat o jejich závislosti explicitně prostřednictvím jejich konstruktory. V některých případech akce jednotlivých kontroleru může vyžadovat služba a nemusí mít smysl k požadavku na úrovni kontroleru. V takovém případě je také možné vložit služby jako parametr v metodě akce.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Vkládání závislostí

Vkládání závislostí je technika, který následuje [závislostí inverzi Princip](http://deviq.com/dependency-inversion-principle/), povolení pro aplikace, musí se skládat z volně párované moduly. Má integrovanou podporu pro ASP.NET Core [vkládání závislostí](../../fundamentals/dependency-injection.md), což usnadňuje aplikace pro testování a údržbu.

## <a name="constructor-injection"></a>Vkládání – konstruktor

ASP.NET Core integrovanou podporu pro vkládání závislostí na základě konstruktor rozšiřuje na řadiče MVC. Stačí přidat typ služby k řadiči jako parametr konstruktoru, se pokusí přeložit typu pomocí jeho vytvořené v kontejneru služby ASP.NET Core. Služby jsou obvykle, ale ne vždy definovány, pomocí rozhraní. Například pokud aplikace obsahuje obchodní logiky, která závisí na aktuální čas, můžete vložit službu, která načte dobu (nikoli nezakódovávejte ho), který by umožnil testy předávat implementace, které používají nastavit čas.

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementace rozhraní tohoto typu tak, aby používala systémové hodiny v době běhu je jednoduchá:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


S tímto na místě můžete použít službu v kontroleru. V takovém případě jsme přidali některé logiku `HomeController` `Index` metodu pro zobrazení pohlednice uživateli podle denní dobu.

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Pokud jsme spustit nyní aplikaci, jsme pravděpodobně dojde k chybě:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

K této chybě dojde, když jsme nenakonfigurovali služby v `ConfigureServices` metoda v našich `Startup` třídy. Chcete-li určit, který požaduje pro `IDateTime` by se měly vyřešit pomocí instance `SystemDateTime`, přidejte zvýrazněný řádek v seznamu níže na vaše `ConfigureServices` metoda:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Tato konkrétní službu může být implementovaná pomocí některé z možností několik různých doba platnosti (`Transient`, `Scoped`, nebo `Singleton`). V tématu [vkládání závislostí](../../fundamentals/dependency-injection.md) pochopit, jak každá z těchto možností oboru ovlivní chování služby.

Jakmile služba byla nakonfigurována, spuštění aplikace a přechodu na domovskou stránku měli zobrazení založené na čase zprávy podle očekávání:

![Server pozdravu](dependency-injection/_static/server-greeting.png)

>[!TIP]
> V tématu [testování logiku řadič](testing.md) Další informace o explicitní žádost o závislosti [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) v řadiče usnadňuje kód pro testování.

Vkládání předdefinovaných závislostí ASP.NET Core podporuje mít jenom jeden konstruktor pro třídy, které žádají o služby. Pokud máte více než jeden konstruktor, může dojít, s oznámením o výjimce:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Jako zobrazí se chybová zpráva, můžete vyřešit potíže s právě jeden konstruktor. Můžete také [nahraďte podpoře vkládání závislostí výchozí implementace třetích stran](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), řadu které budou podporovat několik konstruktorů.

## <a name="action-injection-with-fromservices"></a>Vkládání akce s FromServices

Někdy nepotřebujete služby pro více než jednu akci v rámci vašeho řadiče. V takovém případě má smysl vložení služby jako parametr pro metodu akce. K tomu je potřeba označení parametr s atributem `[FromServices]` jak je vidět tady:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Přístup k nastavení z řadiče

Přístup k aplikaci nebo konfigurace nastavení z v kontroleru je běžný vzor. Tento přístup má použít vzor možnosti popsané v [konfigurace](xref:fundamentals/configuration/index). Nastavení obecně by neměl žádosti přímo z vašeho řadiče pomocí vkládání závislostí. Lepším řešením je požadavek `IOptions<T>` instance, kde `T` je třída konfigurace budete potřebovat.

Chcete-li pracovat s vzoru možnosti, vytvořte třídu, která představuje možnosti, jako je tato:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Pak budete muset nakonfigurovat aplikaci, aby používá možnosti model a přidat vaší třídě konfigurace ke kolekci služby v `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> V seznamu nahoře, jsme konfigurujete aplikaci číst nastavení ze souboru formátu JSON. Zcela v kódu, můžete také nakonfigurovat nastavení, které se zobrazí ve výše uvedeném komentáři kódu. V tématu [konfigurace](xref:fundamentals/configuration/index) pro další možnosti konfigurace.

Jakmile jste určili objekt silného typu konfigurace (v tomto případě `SampleWebSettings`) a jeho přidání ke kolekci služby můžete žádosti ji ze všech Kontroleru nebo metodě akce tím, že požádá o instanci `IOptions<T>` (v tomto případě `IOptions<SampleWebSettings>`) . Následující kód ukazuje, jak jeden vyžadují nastavení z řadiče:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Následující možnosti vzor umožňuje nastavení a konfiguraci, chcete-li být odděleno od sebe navzájem a zajišťuje kontroleru je následující [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/), protože nepotřebuje vědět, jak a kde najít nastavení informace. Také umožňuje kontroleru usnadňují testování částí [testování logiku řadiče](testing.md), protože neexistuje žádné [statické plevami](http://deviq.com/static-cling/) nebo přímé vytváření instancí třídy nastavení v rámci třídy kontroleru.
