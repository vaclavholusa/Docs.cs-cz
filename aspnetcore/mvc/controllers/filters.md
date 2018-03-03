---
title: Filtry
author: ardalis
description: "Zjistěte, jak *filtry* práci a jejich použití v aplikaci ASP.NET MVC jádra."
manager: wpickett
ms.author: tdykstra
ms.date: 12/12/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 5ee2029b3345a76cb283b88da5109ff0d81ebfa4
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="filters"></a>Filtry

Podle [tní Dykstra](https://github.com/tdykstra/) a [Steve Smith](https://ardalis.com/)

*Filtry* v aplikaci ASP.NET MVC základní umožňují spustit kód před nebo po určité fáze v kanálu zpracování požadavků.

 Integrované filtry popisovač úlohy, jako je například autorizace (brání přístupu k prostředkům, které uživatel není oprávněn), zajistíte, že všechny požadavky používat protokol HTTPS a reakce na ukládání do mezipaměti (krátká smyčka kanál požadavku k vrácení odpovědi v mezipaměti). 

Můžete vytvořit vlastní filtry pro zpracování mezi vyjímání otázky pro vaši aplikaci. Kdykoliv budete chtít vyhnout duplikování kód napříč akce, jsou filtry řešení. Můžete například konsolidovat kód ve filtru výjimek pro zpracování chyb.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Jak filtry funguje?

Filtry se spouští v rámci *kanál vyvolání akce MVC*, která se někdy označují jako *filtru kanálu*.  Filtr kanálu spustí po MVC vybere akci provést.

![Další Middleware, směrování Middleware, výběr akce a kanál vyvolání akce MVC zpracování žádosti. Zpracování žádosti pokračuje zpět přes výběr akce, Middleware směrování a různé další Middleware než se stane odpověď může odeslat klientovi.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Typy filtrů

Jednotlivých typů filtrů se spustí v různých fáze v kanálu filtru.

* [Filtry autorizace](#authorization-filters) spouští jako první a slouží k určení, zda je aktuální uživatel oprávnění pro aktuální požadavek. Pokud požadavek není autorizovaný se může krátká smyčka kanálu. 

* [Filtry prostředků](#resource-filters) jsou první, má požadavek zpracovat po autorizaci.  Mohou spouštět kód před rest kanálu filtru, a po dokončení zbytek kanálu. Jsou užitečná implementovat ukládání do mezipaměti nebo jinak krátká smyčka filtru kanálu z důvodů výkonu. Vzhledem k tomu, že se spustit před navázáním modelu, jsou užitečná pro všechny položky, které potřebuje k ovlivnění vazby modelu.

* [Filtry akcí](#action-filters) můžete spustit kód bezprostředně před a po volání metody jednotlivé akce. Můžete se použít k manipulaci s argumenty předávané do akce a výsledku vrácený z akce.

* [Filtry výjimek](#exception-filters) lze použít globální zásady neošetřených výjimek, které udělat předtím, než nic byla zapsána na text odpovědi.

* [Vést filtry](#result-filters) můžete spustit kód bezprostředně před a po provedení výsledky jednotlivé akce. Tyto spustit jenom v případě, že byl úspěšně proveden metody akce a jsou užitečné pro logiku, která musí obklopit zobrazení nebo formátování provádění.

Následující diagram znázorňuje interakci tyto typy filtrů v kanálu filtru.

![Filtry autorizace, filtry prostředků, vazby modelu, filtry akce, provádění akce a akce výsledek převodu, filtry výjimek, filtry výsledků a výsledek spuštění zpracování žádosti. Na cestě se žádost pouze zpracovává filtry výsledků a filtry prostředků než se stane odpověď může odeslat klientovi.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementace

Filtry podporují synchronní a asynchronní implementace prostřednictvím různých rozhraní definice. Vyberte buď synchronizace nebo asynchronní variant v závislosti na druhu úlohy, kterou je třeba provést. 

Synchronní filtry, které můžete spustit kód před a po jejich fáze kanálu definovat na*fáze*zpracování a na*fáze*provést metody. Například `OnActionExecuting` je volána před zavolání metody akce a `OnActionExecuted` je volána po metodě akce vrátí.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Asynchronní filtry definovat jeden na*fáze*ExecutionAsync metoda. Tato metoda přebírá *FilterType*ExecutionDelegate delegáta, který provede fáze kanálu se filtr. Například `ActionExecutionDelegate` volání metody akce a může spustit kód před a po jeho volání.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Můžete implementovat rozhraní pro několik fází filtru do jedné třídy. Například [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) abstraktní třída implementuje obě `IActionFilter` a `IResultFilter`, a také jejich ekvivalenty asynchronní.

> [!NOTE]
> Implementace **buď** synchronní nebo asynchronní verzi rozhraní filtru, nikoli oba dva. Rozhraní framework zkontroluje nejprve Pokud filtr implementuje rozhraní asynchronní, a pokud ano, který volá. Pokud tomu tak není, volá metodu nebo metody rozhraní synchronní. Pokud byste chtěli implementovat obě rozhraní na jednu třídu, by volá asynchronní metody. Při používání abstraktních tříd jako [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) by se mělo přepsat pouze metodu synchronní nebo asynchronní metody pro každý typ filtru.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementuje `IFilter`. Proto `IFilterFactory` instanci SQL lze použít jako `IFilter` instance kdekoli v kanálu filtru. Když rozhraní připraví vyvolání filtr, pokusí se vysílat `IFilterFactory`. Pokud tento přetypování úspěšné, `CreateInstance` metoda je volána k vytvoření `IFilter` instance, která bude volána. To umožňuje návrh s velmi flexibilní, protože kanál přesné filtru nemusí být explicitně nastaveno při spuštění aplikace.

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

Filtr lze přidat do tohoto kanálu v některém z tři *obory*. Pomocí atributu můžete přidat filtr k určité metodě akce nebo třídy kontroleru. Nebo můžete zaregistrovat filtr globálně (pro všechny kontrolery a akce) přidejte jej do `MvcOptions.Filters` kolekce v `ConfigureServices` metoda v `Startup` – třída:

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

Toto pořadí ukazuje, že je metoda filtr vnořených ve filtru řadiče a filtr řadiče vnořen v rámci globálních filtrů. Pro něj jiný způsob, pokud jste uvnitř filtr asynchronní adresy na*fáze*ExecutionAsync metoda všechny filtry náročnější oboru spustit, když kódu v zásobníku.

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

`Order` Vlastnost trumfy oboru při určení pořadí, ve kterém budou spouštět filtry. Filtry jsou seřazeny nejprve podle pořadí a oboru se používá k přerušení ties. Všechny z předdefinovaných filtrů implementovat `IOrderedFilter` a nastavte výchozí `Order` hodnotu na 0, takže obor určuje pořadí, pokud nastavíte `Order` na hodnotu nula.

## <a name="cancellation-and-short-circuiting"></a>Zrušení a krátký circuiting

Může krátká smyčka kanálu filtru v libovolném bodě nastavením `Result` vlastnost `context` zadaný parametr metodu filtru. Filtr prostředků pro instanci nebude zbytek kanálu spuštěn.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

V následujícím kódu jak `ShortCircuitingResourceFilter` a `AddHeader` cílový filtr `SomeResource` metody akce. Ale protože `ShortCircuitingResourceFilter` spustí první (vzhledem k tomu, že je filtr zdroje a `AddHeader` je filtr akce) a zkratům zbytek kanálu, `AddHeader` filtru nikdy běží `SomeResource` akce. Toto chování by být stejné Pokud oba filtry, které byly použity na úrovni metody akce, zadaný `ShortCircuitingResourceFilter` spustil první (z důvodu jeho typ filtru nebo explicitní použití `Order` vlastnost, pro instanci).

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

`ServiceFilterAttribute` implementuje `IFilterFactory`, který zpřístupňuje jedné metody pro vytvoření `IFilter` instance. U `ServiceFilterAttribute`, `IFilterFactory` rozhraní `CreateInstance` je implementovat metodu načtení zadaného typu z kontejneru služby (DI).

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` je velmi podobné `ServiceFilterAttribute` (a také implementuje `IFilterFactory`), ale jeho typ není vyřešit přímo z DI kontejneru. Místo toho vytvoří typ pomocí `Microsoft.Extensions.DependencyInjection.ObjectFactory`.

Vzhledem k tomuto rozdílu typy, které jsou odkazovány pomocí `TypeFilterAttribute` nepotřebují být registrováno v kontejneru, nejdřív (ale budou mít pořád závislé splnil kontejnerem). Navíc `TypeFilterAttribute` může volitelně přijmout argumenty konstruktoru pro daný typ nejistá. Následující příklad ukazuje, jak předat argumenty typu pomocí `TypeFilterAttribute`:

[!code-csharp[](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Pokud máte filtr, který nevyžaduje žádné argumenty, ale který má konstruktor závislosti, které musí být vyplněna podle DI, můžete použít vlastní atribut s názvem na třídy a metody místo `[TypeFilter(typeof(FilterType))]`). Filtr ukazuje, jak to lze provést:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Tento filtr lze použít na třídy nebo metody pomocí `[SampleActionFilter]` syntaxe, místo nutnosti použít `[TypeFilter]` nebo `[ServiceFilter]`.

## <a name="authorization-filters"></a>Filtry autorizace

*Filtry autorizace* řízení přístupu k metody akce a jsou první filtry, které mají být provedeny v rámci kanálu filtru. Mají pouze před metoda, na rozdíl od většiny filtry, které podporují před a po metody. Filtr autorizace by měl zapisovat pouze pokud vytváříte vlastní framework autorizace. Dáváte přednost konfigurace zásad autorizace nebo zápis vlastní zásady autorizace přes psaní vlastního filtru. Integrovaný filtr implementace právě zodpovídá za volání autorizace systému.

Všimněte si, že by neměl throw výjimky v rámci filtry autorizace, vzhledem k tomu, že nic zpracuje výjimku (filtry výjimek nebude zpracování je). Místo toho vydala výzvu nebo jiný způsob, jak najít.

Další informace o [autorizace](../../security/authorization/index.md).

## <a name="resource-filters"></a>Filtry prostředků

*Filtry prostředků* implementovat buď `IResourceFilter` nebo `IAsyncResourceFilter` rozhraní a jejich spuštění zabalí většinu kanálu filtru. (Jenom [filtry autorizace](#authorization-filters) spustit před sebou.) Prostředek filtry jsou obzvláště užitečné, pokud potřebujete krátká smyčka většinu práce, který provádí požadavek. Například se můžete vyhnout ukládání do mezipaměti filtr Pokud odpovědi v mezipaměti zbytek kanálu.

[Krátké circuiting filtr zdroje](#short-circuiting-resource-filter) uvedena výše je příkladem filtr zdroje. Dalším příkladem je [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), která brání vazby modelu z přístupu k datům formuláře. Je užitečné v případě, pokud víte, že se chystáte přijímat nahrávání velkých souborů a chcete zabránit formuláře načítán do paměti.

## <a name="action-filters"></a>Filtry akcí

*Filtry akcí* implementovat buď `IActionFilter` nebo `IAsyncActionFilter` rozhraní a jejich spuštění obklopuje provádění metody akce.

Zde je ukázka filtr akce:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) poskytuje následujících vlastností:

* `ActionArguments` – umožňuje pracovat s vstupů pro akci.
* `Controller` – umožňuje pracovat s instance kontroleru. 
* `Result` -nastavení zkratům provádění metody akce a filtrů následné akce. Došlo k výjimce také zabrání spuštění metody akce a následné filtry, ale je považován za selhání místo úspěšný výsledek.

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) poskytuje `Controller` a `Result` plus následující vlastnosti:

* `Canceled` -bude hodnota true, pokud provádění akce byla zkratována jiný filtr.
* `Exception` -bude obsahovat hodnotu null, pokud akci nebo filtr následné akce došlo k výjimce. Nastavení této vlastnosti na hodnotu null, efektivně 'zpracovává' výjimku, a `Result` bude proveden, jako kdyby se vrátil metody akce normálně.

Pro `IAsyncActionFilter`, volání `ActionExecutionDelegate` provede všechny následné akce filtry a metoda akce vrací `ActionExecutedContext`. Krátká smyčka, přiřaďte `ActionExecutingContext.Result` na některé výsledek instance a nemůžete volat `ActionExecutionDelegate`.

Rozhraní framework poskytuje abstraktní `ActionFilterAttribute` , ke kterým můžete podtřídy. 

Filtr akce můžete automaticky ověřit stav modelu a vrátí případné chyby, pokud stav je neplatný:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

`OnActionExecuted` Metoda spustí po metody akce a může zobrazit a pracovat s výsledky akce prostřednictvím `ActionExecutedContext.Result` vlastnost. `ActionExecutedContext.Canceled` bude nastavena na hodnotu true, pokud provádění akce byla zkratována jiný filtr. `ActionExecutedContext.Exception` bude být nastavená na hodnotu jinou hodnotu než null, pokud akci nebo filtr následné akce došlo k výjimce. Nastavení `ActionExecutedContext.Exception` na hodnotu null, efektivně 'zpracovává' výjimku a `ActionExectedContext.Result` bude proveden pak jako v případě, že ho byly obvykle vrátila z metody akce.

## <a name="exception-filters"></a>Filtry výjimek

*Filtry výjimek* implementovat buď `IExceptionFilter` nebo `IAsyncExceptionFilter` rozhraní. Jejich lze použít k implementaci běžných Chyba při zpracování zásady pro aplikace. 

Filtr výjimek následující ukázka používá vlastní vývojáře chyba zobrazení si můžete zobrazit podrobnosti o výjimkách, ke kterým dochází, když je aplikace v vývoj:

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Filtry výjimek nemají dvě události (pro před a po) – jenom implementovat `OnException` (nebo `OnExceptionAsync`). 

Filtry výjimek zpracování neošetřených výjimek, které se vyskytují ve vytváření řadiče [model vazby](../models/model-binding.md), filtrů Akce nebo metody akce. Jejich nezachytí výjimky, které se vyskytují v prostředku filtry, filtry výsledků nebo MVC výsledek spuštění.

Ke zpracování výjimky, nastavte `ExceptionContext.ExceptionHandled` vlastnost na hodnotu true nebo zápisu odpovědi. To zastaví šíření výjimky. Všimněte si, že filtr výjimek nelze zapnout výjimku do "ÚSPĚCH". Pouze filtr akce můžete udělat.

> [!NOTE]
> V technologii ASP.NET 1.1 odpovědi neposílají, pokud jste nastavili `ExceptionHandled` na hodnotu true **a** zápisu odpovědi. V tomto scénáři ASP.NET Core 1.0 odeslání odpovědi a ASP.NET Core 1.1.2 vrátí 1.0 chování. Další informace najdete v tématu [vydání #5594](https://github.com/aspnet/Mvc/issues/5594) v úložišti GitHub. 

Filtry výjimek jsou vhodné pro soutisku výjimky, které se vyskytují v akce MVC, jsou ale není tak účinná jako chyba zpracování middleware. Dáváte přednost middleware pro obecné případ a použít filtry, pouze pokud potřebujete udělat zpracování chyb *jinak* podle MVC akci, která jste vybrali. Aplikace může mít například metody akce pro obě rozhraní API koncových bodů a zobrazení nebo HTML. Koncové body rozhraní API může vrátí informace o chybě formátu JSON, zatímco akce na základě zobrazení může vrátit chybovou stránku ve formátu HTML.

Rozhraní framework poskytuje abstraktní `ExceptionFilterAttribute` , ke kterým můžete podtřídy. 

## <a name="result-filters"></a>Filtry výsledků

*Vést filtry* implementovat buď `IResultFilter` nebo `IAsyncResultFilter` rozhraní a jejich spuštění obklopuje provádění výsledky akce. 

Tady je příklad výsledek filtr, který přidá hlavičku protokolu HTTP.

[!code-csharp[](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Typ výsledku spouštěna, závisí na dotyčném akce. Akce MVC vrací zobrazení by mělo zahrnovat všechny razor zpracování v rámci `ViewResult` spouštěna. Metody rozhraní API může provést některé serializace jako součást spuštění výsledku. Další informace o [výsledky akce](actions.md)

Filtry výsledků jsou pro úspěšné výsledky - spustit pouze když akce nebo filtrů akce vytvoří výsledek akce. Filtry výsledků nejsou spustit, když filtry výjimek zpracování výjimky.

`OnResultExecuting` Metoda může krátká smyčka – spouštění výsledek akce a následné výsledek filtry nastavením `ResultExecutingContext.Cancel` na hodnotu true. By měl obvykle zapisovat do objektu odpovědi při krátká smyčka, aby nedošlo ke generování prázdnou odpověď. Došlo k výjimce také zabrání spuštění výsledku akce a následné filtry, ale bude považována za selhání místo úspěšný výsledek.

Když `OnResultExecuted` metoda spustí, odpověď pravděpodobně byl odeslán do klienta a nelze změnit další (Pokud byla vyvolána výjimka). `ResultExecutedContext.Canceled` bude nastavena na hodnotu true, pokud provádění výsledek akce se zkratována jiný filtr.

`ResultExecutedContext.Exception` bude být nastavená na hodnotu jinou hodnotu než null, pokud výsledek akce nebo filtr následné výsledků došlo k výjimce. Nastavení `Exception` na hodnotu null efektivně zpracovává výjimku a brání výjimky z se znovu vyvolány podle MVC později v kanálu. Pokud jste zpracování výjimek v filtr výsledků, nemusí být možné zapisovat data do odpovědi. Pokud výsledek akce, vyvolá partway prostřednictvím jejího provádění a hlavičky již byly vyprázdněny klientovi, neexistuje žádný spolehlivý mechanismus odeslat kód chyby.

Pro `IAsyncResultFilter` volání `await next()` na `ResultExecutionDelegate` provede všechny následné výsledek filtry a výsledku akce. Krátká smyčka, nastavte `ResultExecutingContext.Cancel` na hodnotu true a nemůžete volat `ResultExectionDelegate`.

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
