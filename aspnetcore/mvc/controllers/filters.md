---
title: Filtry v ASP.NET Core
author: ardalis
description: Zjistěte, jak fungují filtry a jejich použití v aplikaci ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 4/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 49e51a867e47ce375a5048cae5979360c4103365
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/27/2018
---
# <a name="filters-in-aspnet-core"></a>Filtry v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra/), a [Steve Smith](https://ardalis.com/)

*Filtry* v aplikaci ASP.NET MVC základní umožňují spustit kód před nebo po konkrétní fáze v kanálu zpracování požadavků.

> [!IMPORTANT]
> Toto téma neobsahuje **není** platí pro stránky Razor. ASP.NET Core 2.1 a novější podporuje [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) a [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) pro stránky Razor. Další informace najdete v tématu [filtrovat metody pro stránky Razor](xref:mvc/razor-pages/filter).

 Integrované filtry zpracování úloh, jako:
 
 * Autorizace (brání přístupu k prostředkům, které uživatel není oprávněn).
 * Zajištění, že všechny požadavky používat protokol HTTPS.
 * Odpověď ukládání do mezipaměti (krátká smyčka kanál požadavku k vrácení odpovědi v mezipaměti). 

Vlastní filtry lze vytvořit pro zpracování mezi vyjímání otázky. Filtry se můžete vyhnout duplikování kód napříč akce. Zpracování filtru výjimek chyby může například konsolidovat zpracování chyb.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Jak filtry funguje?

Filtry se spouští v rámci *kanál vyvolání akce MVC*, která se někdy označují jako *filtru kanálu*.  Filtr kanálu spustí po MVC vybere akci provést.

![Další Middleware, směrování Middleware, výběr akce a kanál vyvolání akce MVC zpracování žádosti. Zpracování žádosti pokračuje zpět přes výběr akce, Middleware směrování a různé další Middleware než se stane odpověď může odeslat klientovi.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Typy filtrů

Jednotlivých typů filtrů se spustí v různých fáze v kanálu filtru.

* [Filtry autorizace](#authorization-filters) spouští jako první a slouží k určení, zda je aktuální uživatel oprávnění pro aktuální požadavek. Pokud požadavek není autorizovaný se může krátká smyčka kanálu. 

* [Filtry prostředků](#resource-filters) jsou první, má požadavek zpracovat po autorizaci.  Mohou spouštět kód před rest kanálu filtru, a po dokončení zbytek kanálu. Jsou užitečná implementovat ukládání do mezipaměti nebo jinak krátká smyčka filtru kanálu z důvodů výkonu. Se spustit před vazby modelu, takže se mohou mít vliv na vazby modelu.

* [Filtry akcí](#action-filters) můžete spustit kód bezprostředně před a po volání metody jednotlivé akce. Můžete se použít k manipulaci s argumenty předávané do akce a výsledku vrácený z akce.

* [Filtry výjimek](#exception-filters) lze použít globální zásady neošetřených výjimek, které udělat předtím, než nic byla zapsána na text odpovědi.

* [Vést filtry](#result-filters) můžete spustit kód bezprostředně před a po provedení výsledky jednotlivé akce. Se spustí pouze v případě, že byl úspěšně proveden metody akce. Jsou užitečné pro logiku, která musí obklopit spuštění zobrazení nebo formátování.

Následující diagram znázorňuje interakci tyto typy filtrů v kanálu filtru.

![Filtry autorizace, filtry prostředků, vazby modelu, filtry akce, provádění akce a akce výsledek převodu, filtry výjimek, filtry výsledků a výsledek spuštění zpracování žádosti. Na cestě se žádost pouze zpracovává filtry výsledků a filtry prostředků než se stane odpověď může odeslat klientovi.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementace

Filtry podporují synchronní a asynchronní implementace prostřednictvím různých rozhraní definice. 

Synchronní filtry, které můžete spustit kód před a po jejich fáze kanálu definovat na*fáze*zpracování a na*fáze*provést metody. Například `OnActionExecuting` je volána před zavolání metody akce a `OnActionExecuted` je volána po metodě akce vrátí.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet1)]

Asynchronní filtry definovat jeden na*fáze*ExecutionAsync metoda. Tato metoda přebírá *FilterType*ExecutionDelegate delegáta, který provede fáze kanálu se filtr. Například `ActionExecutionDelegate` volání metody akce nebo další akce filtru a může spustit kód s před a po jeho volání.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Můžete implementovat rozhraní pro několik fází filtru do jedné třídy. Například [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) třída implementuje `IActionFilter`, `IResultFilter`a jejich ekvivalenty asynchronní.

> [!NOTE]
> Implementace **buď** synchronní nebo asynchronní verzi rozhraní filtru, nikoli oba dva. Rozhraní framework zkontroluje nejprve Pokud filtr implementuje rozhraní asynchronní, a pokud ano, který volá. Pokud tomu tak není, volá metodu nebo metody rozhraní synchronní. Pokud byste chtěli implementovat obě rozhraní na jednu třídu, by volá asynchronní metody. Při používání abstraktních tříd jako [ActionFilterAttribute](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute?view=aspnetcore-2.0) by se mělo přepsat pouze metodu synchronní nebo asynchronní metody pro každý typ filtru.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementuje `IFilter`. Proto `IFilterFactory` instanci SQL lze použít jako `IFilter` instance kdekoli v kanálu filtru. Když rozhraní připraví vyvolání filtr, pokusí se vysílat `IFilterFactory`. Pokud tento přetypování úspěšné, `CreateInstance` metoda je volána k vytvoření `IFilter` instance, která bude volána. To poskytuje flexibilní návrhu, protože kanál přesné filtru nemusí být explicitně nastaveno při spuštění aplikace.

Můžete implementovat `IFilterFactory` na vlastní atribut implementace jako další postup pro vytvoření filtrů:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Integrovaný filtr atributy

Rozhraní framework zahrnuje integrované filtry na základě atributů, můžete podtřídami a přizpůsobit. Například následující filtr výsledků přidá hlavičku odpovědi.

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Atributy Povolit filtry tak, aby přijímal argumenty, jak je znázorněno v příkladu nahoře. By přidat tento atribut do metody kontroler nebo akce a zadejte název a hodnotu záhlaví HTTP:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Výsledkem `Index` akce jsou uvedeny níže – hlavičky odpovědi jsou zobrazené na vpravo dole.

![Vývojáře nástroje Microsoft Edge zobrazující hlavičky odpovědi, včetně Autor Steve Smith @ardalis](filters/_static/add-header.png)

Některé z rozhraní filtru mít odpovídající atributy, které můžete použít jako základní třídy pro vlastní implementace.

Atributy filtru:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` a `ServiceFilterAttribute` jsou vysvětleny [dále v tomto článku](#dependency-injection).

## <a name="filter-scopes-and-order-of-execution"></a>Oborů filtru a pořadí provádění

Filtr lze přidat do tohoto kanálu v některém z tři *obory*. Pomocí atributu můžete přidat filtr k určité metodě akce nebo třídy kontroleru. Nebo můžete zaregistrovat filtr globálně pro všechny kontrolery a akce. Filtry jsou přidávány globálně jejím přidáním do `MvcOptions.Filters` kolekce v `ConfigureServices`:

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Výchozí pořadí provádění

Pokud je pro konkrétní fáze kanálu více filtrů, obor Určuje výchozí pořadí spuštění filtrů.  Globální filtry obklopit třída filtrů, které pak obklopit metoda filtry. To je někdy označuje jako "Ruština panenky" vnoření jako každé zvýšení rozsahu zabalená kolem předchozí oboru, jako je [vnoření panenky](https://wikipedia.org/wiki/Matryoshka_doll). Toto chování žádoucí přepsání obecně získáte bez nutnosti explicitně určit řazení.

V důsledku této vnoření *po* spuštění kódu filtrů v obráceném pořadí, než *před* kódu. Pořadí vypadá takto:

* *Před* kód filtry, které jsou použity globálně
  * *Před* kód filtry u řadiče
    * *Před* kód filtry u metody akce
    * *Po* kód filtry u metody akce
  * *Po* kód filtry u řadiče
* *Po* kód filtry, které jsou použity globálně
  
Tady je příklad, který znázorňuje pořadí, ve které filtru se nazývají metody pro synchronní filtrů akce.

| Pořadí | Obor filtru | Filter – metoda |
|:--------:|:------------:|:-------------:|
| 1 | Globální | `OnActionExecuting` |
| 2 | Řadiče | `OnActionExecuting` |
| 3 | Metoda | `OnActionExecuting` |
| 4 | Metoda | `OnActionExecuted` |
| 5 | Řadiče | `OnActionExecuted` |
| 6 | Globální | `OnActionExecuted` |

Zobrazí se toto pořadí:

* Metoda filtr je vnořených ve filtru řadiče.
* Filtr řadiče vnořen v rámci globálních filtrů. 

Pro něj jiný způsob, pokud jste uvnitř filtr asynchronní adresy na*fáze*ExecutionAsync metoda všechny filtry náročnější oboru spustit, když kódu v zásobníku.

> [!NOTE]
> Každý řadič, která dědí z `Controller` základní třída zahrnuje `OnActionExecuting` a `OnActionExecuted` metody. Tyto metody zabalení používající filtry pro danou akci: `OnActionExecuting` je volána před provedením filtry, a `OnActionExecuted` je volána po všechny filtry.

### <a name="overriding-the-default-order"></a>Přepsání výchozího pořadí

Můžete přepsat výchozí pořadí provádění implementací `IOrderedFilter`. Toto rozhraní zpřístupní `Order` vlastnost, která má přednost před oboru k určení pořadí zpracování. Filtr s nižší `Order` bude mít hodnotu jeho *před* kód spustit před filtru s vyšší hodnotou `Order`. Filtr s nižší `Order` bude mít hodnotu jeho *po* kód provést, po který filtr s vyšším `Order` hodnotu. Můžete nastavit `Order` vlastnost pomocí parametr konstruktoru:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Pokud máte stejné 3 filtrů akce uvedené v předchozí příklad ale sadu `Order` vlastnost řadiče a globální filtry na 1 a 2 v uvedeném pořadí, pořadí provádění by být změněn na opačný.

| Pořadí | Obor filtru | `Order` Vlastnost | Filter – metoda |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Metoda | 0 | `OnActionExecuting` |
| 2 | Řadiče | 1  | `OnActionExecuting` |
| 3 | Globální | 2  | `OnActionExecuting` |
| 4 | Globální | 2  | `OnActionExecuted` |
| 5 | Řadiče | 1  | `OnActionExecuted` |
| 6 | Metoda | 0  | `OnActionExecuted` |

`Order` Vlastnost trumfy oboru při určení pořadí, ve kterém budou spouštět filtry. Filtry jsou seřazeny nejprve podle pořadí a oboru se používá k přerušení ties. Všechny z předdefinovaných filtrů implementovat `IOrderedFilter` a nastavte výchozí `Order` hodnotu na 0. Rozsah pro integrované filtry, určuje pořadí, není-li nastavena `Order` na hodnotu nula.

## <a name="cancellation-and-short-circuiting"></a>Zrušení a krátký circuiting

Může krátká smyčka kanálu filtru v libovolném bodě nastavením `Result` vlastnost `context` zadaný parametr metodu filtru. Filtr prostředků pro instanci nebude zbytek kanálu spuštěn.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

V následujícím kódu jak `ShortCircuitingResourceFilter` a `AddHeader` cílový filtr `SomeResource` metody akce. `ShortCircuitingResourceFilter`:

* Spustí první, protože je filtr zdroje a `AddHeader` je filtr akce.
* Zkratům zbytek kanálu.

Proto `AddHeader` filtru nikdy běží `SomeResource` akce. Toto chování by být stejné Pokud oba filtry, které byly použity na úrovni metody akce, zadaný `ShortCircuitingResourceFilter` spustil první. `ShortCircuitingResourceFilter` Spustí první z důvodu jeho typ filtru nebo pomocí explicitní `Order` vlastnost.

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Vkládání závislostí

Filtry je možné přidat podle typu nebo instance. Pokud chcete přidat instance, tato instance se použije pro každý požadavek. Pokud přidáte typu, bude typ aktivované, což znamená, vytvoří se instance pro každý požadavek, a všechny závislosti konstruktor vyplní podle [vkládání závislostí](../../fundamentals/dependency-injection.md) (DI). Přidání filtru podle typu je ekvivalentní `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Filtry, které jsou implementovány jako atributy a přidat přímo do třídy kontroleru nebo metody akce nemohou mít závislosti konstruktor poskytované [vkládání závislostí](../../fundamentals/dependency-injection.md) (DI). Je to proto, že atributy musí mít své konstruktor parametry zadané, kde uplatňují. Jedná se o omezení o fungování atributy.

Pokud vaše filtrů závislosti, které potřebujete získat přístup z DI, existuje několik podporované přístupů. Vašemu filtru můžete použít pro třídu nebo akce metoda pomocí jedné z následujících akcí:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` na vaše atribut provedena.

> [!NOTE]
> Závislostí, které chcete získat z DI je protokolovacího nástroje. Však vyhnout se vytváření a použití filtrů výhradně pro účely protokolování, protože [funkce protokolování vestavěnou architekturou](xref:fundamentals/logging/index) již poskytnout co potřebujete. Pokud se chystáte přidat protokolování do filtry, měli zaměřit na obchodní domény obavy nebo chování, které jsou specifické pro filtr, než akce MVC nebo dalších událostí, framework.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

A `ServiceFilter` načte z DI instance filtru. Přidejte filtr do kontejneru v `ConfigureServices`a v odkazovat `ServiceFilter` atribut

[!code-csharp[](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Pomocí `ServiceFilter` bez registrace výsledky typu filtru výjimku:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

`ServiceFilterAttribute` implementuje `IFilterFactory`. `IFilterFactory` zpřístupní `CreateInstance` metodu vytváření `IFilter` instance. `CreateInstance` Metoda načte zadaného typu z kontejneru služby (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` je podobná `ServiceFilterAttribute`, ale jeho typ není vyřešit přímo z DI kontejneru. Vytvoření instance typu pomocí `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Tento rozdíl:

* Typy, které jsou odkazovány pomocí `TypeFilterAttribute` nepotřebují být registrováno v kontejneru, nejdřív.  Mají závislé splnit kontejneru. 
* `TypeFilterAttribute` Volitelně můžete přijmout argumenty konstruktoru typu. 

Následující příklad ukazuje, jak předat argumenty typu pomocí `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Pokud máte filtr který:

* Nevyžaduje žádné argumenty.
* Má konstruktor závislosti, které musí být vyplněna podle DI.

Můžete použít vlastní atribut s názvem na třídy a metody místo `[TypeFilter(typeof(FilterType))]`). Filtr ukazuje, jak to lze provést:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Tento filtr lze použít na třídy nebo metody pomocí `[SampleActionFilter]` syntaxe, místo nutnosti použít `[TypeFilter]` nebo `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtry autorizace

* Filtry autorizace:
* Řízení přístupu na metody akce.
* Jsou první filtry k provést v rámci kanálu filtru. 
* Mít před metoda, ale ne po metodě. 

Filtr autorizace by měl zapisovat pouze pokud vytváříte vlastní framework autorizace. Dáváte přednost konfigurace zásad autorizace nebo zápis vlastní zásady autorizace přes psaní vlastního filtru. Integrovaný filtr implementace právě zodpovídá za volání autorizace systému.

Výjimky v rámci filtry autorizace, by neměl výjimku, protože nic zpracuje výjimku (filtry výjimek nebude zpracování je). Zvažte vydání výzvu, když dojde k výjimce.

Další informace o [autorizace](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtry prostředků

* Implementovat buď `IResourceFilter` nebo `IAsyncResourceFilter` rozhraní
* Jejich provádění zabalí většinu kanálu filtru. 
* Pouze [filtry autorizace](#authorization-filters) spustit před filtry prostředků.

Prostředek filtry jsou vhodné pro většinu práce, který provádí požadavek krátká smyčka. Například se můžete vyhnout ukládání do mezipaměti filtr Pokud odpovědi v mezipaměti zbytek kanálu.

[Krátké circuiting filtr zdroje](#short-circuiting-resource-filter) uvedena výše je příkladem filtr zdroje. Dalším příkladem je [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):

* Zabrání vazby modelu z přístupu k datům formuláře. 
* To je užitečné pro nahrávání velkých souborů a chcete zabránit formuláře načítán do paměti.

## <a name="action-filters"></a>Filtry akcí

*Filtry akcí*:

* Implementovat buď `IActionFilter` nebo `IAsyncActionFilter` rozhraní.
* Jejich provádění obklopuje provádění metody akce.

Zde je ukázka filtr akce:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) poskytuje následujících vlastností:

* `ActionArguments` – umožňuje pracovat s vstupů pro akci.
* `Controller` – umožňuje pracovat s instance kontroleru. 
* `Result` -nastavení zkratům provádění metody akce a filtrů následné akce. Došlo k výjimce také zabrání spuštění metody akce a následné filtry, ale je považován za selhání místo úspěšný výsledek.

[ActionExecutedContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) poskytuje `Controller` a `Result` plus následující vlastnosti:

* `Canceled` -bude hodnota true, pokud provádění akce byla zkratována jiný filtr.
* `Exception` -bude obsahovat hodnotu null, pokud akci nebo filtr následné akce došlo k výjimce. Nastavení této vlastnosti na hodnotu null, efektivně 'zpracovává' výjimku, a `Result` bude proveden, jako kdyby se vrátil metody akce normálně.

Pro `IAsyncActionFilter`, volání `ActionExecutionDelegate`:

* Zpracuje všechny následné akce filtry a metody akce.
* Vrátí `ActionExecutedContext`. 

Krátká smyčka, přiřaďte `ActionExecutingContext.Result` na některé výsledek instance a nemůžete volat `ActionExecutionDelegate`.

Rozhraní framework poskytuje abstraktní `ActionFilterAttribute` , ke kterým můžete podtřídy. 

Filtr akce můžete použít k ověření stavu modelu a vrátí případné chyby, pokud stav je neplatný:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Metoda spustí po metody akce a může zobrazit a pracovat s výsledky akce prostřednictvím `ActionExecutedContext.Result` vlastnost. `ActionExecutedContext.Canceled` bude nastavena na hodnotu true, pokud provádění akce byla zkratována jiný filtr. `ActionExecutedContext.Exception` bude být nastavená na hodnotu jinou hodnotu než null, pokud akci nebo filtr následné akce došlo k výjimce. Nastavení `ActionExecutedContext.Exception` na hodnotu null:

* Efektivně 'zpracovává' výjimku.
* `ActionExectedContext.Result` se spustí, jako kdyby byly obvykle vrátila z metody akce.

## <a name="exception-filters"></a>Filtry výjimek

*Filtry výjimek* implementovat buď `IExceptionFilter` nebo `IAsyncExceptionFilter` rozhraní. Jejich lze použít k implementaci běžných Chyba při zpracování zásady pro aplikace. 

Filtr výjimek následující ukázka používá vlastní vývojáře chyba zobrazení si můžete zobrazit podrobnosti o výjimkách, které nastat, pokud je při vývoji aplikace:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtry výjimek:

* Nemáte před a po události. 
* Implementace `OnException` nebo `OnExceptionAsync`. 
* Zpracování neošetřených výjimek, které se vyskytují ve vytváření řadiče [model vazby](../models/model-binding.md), filtrů Akce nebo metody akce. 
* Zachytávat výjimky, které se vyskytují v prostředku filtry, filtry výsledků nebo MVC výsledek spuštění.

Ke zpracování výjimky, nastavte `ExceptionContext.ExceptionHandled` vlastnost na hodnotu true nebo zápisu odpovědi. To zastaví šíření výjimky. Filtr výjimek nelze zapnout výjimku do "ÚSPĚCH". Pouze filtr akce můžete udělat.

> [!NOTE]
> V technologii ASP.NET Core 1.1, odpověď neposílají, pokud jste nastavili `ExceptionHandled` na hodnotu true **a** zápisu odpovědi. V tomto scénáři ASP.NET Core 1.0 odeslání odpovědi a ASP.NET Core 1.1.2 vrátí 1.0 chování. Další informace najdete v tématu [vydání #5594](https://github.com/aspnet/Mvc/issues/5594) v úložišti GitHub. 

Filtry výjimek:

* Jsou vhodné pro zachytávání výjimek, které se vyskytují v akce MVC.
* Nejsou tak účinná jako chyba zpracování middleware. 

Dáváte přednost middleware pro zpracování výjimek. Použít filtry výjimek, pouze pokud musíte udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali. Aplikace může mít například metody akce pro obě rozhraní API koncových bodů a zobrazení nebo HTML. Koncové body rozhraní API může vrátí informace o chybě formátu JSON, zatímco akce na základě zobrazení může vrátit chybovou stránku ve formátu HTML.

`ExceptionFilterAttribute` Můžete rozčlenění. 

## <a name="result-filters"></a>Filtry výsledků

* Implementovat buď `IResultFilter` nebo `IAsyncResultFilter` rozhraní.
* Jejich provádění obklopuje provádění výsledky akce. 

Tady je příklad výsledek filtr, který přidá hlavičku protokolu HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Typ výsledku spouštěna, závisí na dotyčném akce. Akce MVC vrací zobrazení by mělo zahrnovat všechny razor zpracování v rámci `ViewResult` spouštěna. Metody rozhraní API může provést některé serializace jako součást spuštění výsledku. Další informace o [výsledky akce](actions.md)

Filtry výsledků jsou pro úspěšné výsledky - spustit pouze když akce nebo filtrů akce vytvoří výsledek akce. Filtry výsledků nejsou spustit, když filtry výjimek zpracování výjimky.

`OnResultExecuting` Metoda může krátká smyčka – spouštění výsledek akce a následné výsledek filtry nastavením `ResultExecutingContext.Cancel` na hodnotu true. By měl obvykle zapisovat do objektu odpovědi při krátká smyčka, aby nedošlo ke generování prázdnou odpověď. Vyvolání dojde k výjimce:

* Zabránit spouštění výsledek akce a následné filtry.
* Jednal jako selhání místo úspěšný výsledek.

Když `OnResultExecuted` metoda spustí, odpověď pravděpodobně byl odeslán do klienta a nelze změnit další (Pokud byla vyvolána výjimka). `ResultExecutedContext.Canceled` bude nastavena na hodnotu true, pokud provádění výsledek akce se zkratována jiný filtr.

`ResultExecutedContext.Exception` bude být nastavená na hodnotu jinou hodnotu než null, pokud výsledek akce nebo filtr následné výsledků došlo k výjimce. Nastavení `Exception` na hodnotu null efektivně zpracovává výjimku a brání výjimky z se znovu vyvolány podle MVC později v kanálu. Pokud jste zpracování výjimek v filtr výsledků, nemusí být možné zapisovat data do odpovědi. Pokud výsledek akce, vyvolá partway prostřednictvím jejího provádění a hlavičky již byly vyprázdněny klientovi, neexistuje žádný spolehlivý mechanismus odeslat kód chyby.

Pro `IAsyncResultFilter` volání `await next` na `ResultExecutionDelegate` provede všechny následné výsledek filtry a výsledku akce. Krátká smyčka, nastavte `ResultExecutingContext.Cancel` na hodnotu true a nemůžete volat `ResultExectionDelegate`.

Rozhraní framework poskytuje abstraktní `ResultFilterAttribute` , ke kterým můžete podtřídy. [AddHeaderAttribute](#add-header-attribute) třída uvedena výše je příkladem atribut filtru výsledků.

## <a name="using-middleware-in-the-filter-pipeline"></a>Pomocí middleware v kanálu filtru

Filtry prostředků fungují jako [middleware](xref:fundamentals/middleware/index) v tom, že jejich obklopit provádění všechny položky, která se dodává později v kanálu. Ale filtry se liší od middleware, že jsou součástí MVC, což znamená, že mají přístup do kontextu MVC a konstrukce.

V technologii ASP.NET Core 1.1 můžete použít middleware v kanálu filtru. Můžete chtít udělat, pokud máte komponenta middlewaru, který potřebuje přístup k data směrování MVC nebo ten, který by měly být spuštěny pouze pro určité řadiče nebo akce.

Pokud chcete použít jako filtr middleware, vytvoření typu s `Configure` metoda, která určuje middleware, který chcete vložit do kanálu filtru. Tady je příklad, který používá lokalizace middleware pro aktuální jazykovou verzi pro žádost o vytvoření:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Pak můžete použít `MiddlewareFilterAttribute` ke spuštění middleware pro vybrané kontroler nebo akce nebo globálně:

[!code-csharp[](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Filtry middleware ve fázi stejné filtru kanálu jako prostředek filtry, před navázáním modelu a po zbytek kanálu.

## <a name="next-actions"></a>Další akce

A experimentovat s filtry, [stáhnout, otestovat a upravit ukázku](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).
