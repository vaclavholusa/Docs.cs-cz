---
title: "Vzor možnosti v ASP.NET Core"
author: guardrex
description: "Zjistit, jak používat možnosti vzor k reprezentování skupiny související nastavení v aplikacích ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: abb3b92af07a7b3b199712fcfdc459ca283d0017
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="options-pattern-in-aspnet-core"></a>Vzor možnosti v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

Vzor možnosti používá k reprezentování skupiny související nastavení možnosti třídy. Při nastavení konfigurace jsou izolované funkce do třídy samostatné možnosti, aplikace dodržuje zásady dvě důležité inženýrství softwaru:

* [Rozhraní oddělení princip (ISP)](http://deviq.com/interface-segregation-principle/): funkce (třídy), které závisí na nastavení konfigurace závisí na nastavení konfigurace, která používají jenom.
* [Oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/): nastavení pro různé části aplikace nejsou závislé nebo párované jednu na druhou.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample)) Tento článek je snazší postupujte podle ukázkovou aplikaci.

## <a name="basic-options-configuration"></a>Základní možnosti konfigurace

Základní možnosti konfigurace je znázorněn příklad &num;1 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Třída možností musí být neabstraktní s konstruktor public bez parametrů. Následující třídy, `MyOptions`, má dvě vlastnosti `Option1` a `Option2`. Nastavení výchozích hodnot je nepovinný, ale konstruktoru třídy v následujícím příkladu se nastaví na výchozí hodnotu `Option1`. `Option2`Výchozí hodnota je nastavena pomocí vlastnosti inicializace přímo (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Třída je přidat do kontejneru služby s [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) a vázaný k konfigurace:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

Následující stránka používá model [vkládání závislostí konstruktor](xref:fundamentals/dependency-injection#what-is-dependency-injection) s [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) pro přístup k nastavení (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Ukázkových *appSettings.JSON určený* soubor Určuje hodnoty pro `option1` a `option2`:

[!code-json[Main](options/sample/appsettings.json)]

Pokud aplikace běží a model stránky `OnGet` metoda vrátí řetězec zobrazující třída hodnoty možnosti:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Jednoduché možnosti nakonfigurovat delegáta

Jednoduché možnosti konfigurace s delegáta je znázorněn příklad &num;2 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Použití delegáta pro nastavení možností hodnot. Použití ukázkové aplikace `MyOptionsWithDelegateConfig` – třída (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

V následujícím kódu druhý `IConfigureOptions<TOptions>` služby se přidá do kontejneru služby. Ke konfiguraci vazby se používá delegáta `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Můžete přidat několik poskytovatelů konfigurace. Poskytovatelé konfigurace jsou k dispozici v balíčky NuGet. Uplatňují se, aby bylo úspěšné registrace.

Každé volání [konfigurace&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) přidá `IConfigureOptions<TOptions>` službu kontejneru služby. V předchozím příkladu, hodnoty `Option1` a `Option2` jsou určené v *appSettings.JSON určený*, ale hodnoty `Option1` a `Option2` jsou přepsány nakonfigurované delegáta.

Pokud je povoleno více než jedna služba konfigurace, poslední zdroj konfigurace specifikovat *wins* a nastaví hodnotu konfigurace. Pokud aplikace běží a model stránky `OnGet` metoda vrátí řetězec zobrazující třída hodnoty možnosti:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Konfigurace suboptions

Konfigurace suboptions je znázorněn příklad &num;3 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Aplikace by měl vytvořit možnosti třídy, které se vztahují k příslušné funkce skupiny (třídy) v aplikaci. Části aplikace, které vyžadují hodnoty konfigurace pouze měli přístup k hodnotám konfigurace, které používají.

Při vytváření vazby možnosti konfigurace, každou vlastnost v možnosti typ je vázán na konfigurační klíč ve formátu `property[:sub-property:]`. Například `MyOptions.Option1` vlastnost je vázána na klíč `Option1`, který je pro čtení z `option1` vlastnost *appSettings.JSON určený*.

V následujícím kódu, třetí `IConfigureOptions<TOptions>` služby se přidá do kontejneru služby. Vytvoří vazbu mezi `MySubOptions` do části `subsection` z *appSettings.JSON určený* souboru:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` Rozšíření metoda vyžaduje, [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) balíček NuGet. Pokud aplikace používá [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, tento balíček je automaticky zahrnuty.

Ukázkových *appSettings.JSON určený* soubor definuje `subsection` člena s klíči pro `suboption1` a `suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` Třída definuje vlastnosti, `SubOption1` a `SubOption2`, aby udržení hodnoty dílčí možnosti (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

Model stránky `OnGet` metoda vrátí řetězec s hodnotami dílčí možnost (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Při spuštění aplikace `OnGet` metoda vrátí řetězec zobrazující dílčí možnost hodnoty třídy:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Možnosti poskytnuté modelem zobrazení nebo pomocí vkládání přímé zobrazení

Možnosti poskytnuté modelem zobrazení nebo pomocí vkládání přímé zobrazení je znázorněn příklad &num;4 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Možnosti můžete zadat v zobrazení modelu nebo vložením `IOptions<TOptions>` přímo do zobrazení (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Přímé vkládání vložit `IOptions<MyOptions>` s `@inject` – direktiva:

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Při spuštění aplikace na vykreslené stránce se zobrazuje hodnoty možnosti:

![Možnosti hodnoty možnost 1: value1_from_json a Option2: -1 se načtou z modelu a vkládání do zobrazení.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Znovu načíst data konfigurace s IOptionsSnapshot

Opětovné načtení konfiguračních dat pomocí `IOptionsSnapshot` je znázorněn v příkladu &num;5 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Vyžaduje ASP.NET Core 1.1 nebo novější.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) podporuje opětovném načtení možnosti s minimální režie zpracování. V technologii ASP.NET Core 1.1 `IOptionsSnapshot` je snímek [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) a aktualizace automaticky vždy, když monitorování aktivuje na základě změny zdroj dat změny. V technologii ASP.NET Core 2.0 nebo novější možnosti se vypočítávají jednou za žádosti při přístup a uložené v mezipaměti po dobu jeho existence požadavku.

Následující příklad ukazuje, jak novou `IOptionsSnapshot` je vytvořen po *appSettings.JSON určený* změny (*Pages/Index.cshtml.cs*). Víc požadavků na server vrátit konstantní hodnoty poskytované *appSettings.JSON určený* souborů, dokud se změní soubor a konfigurace znovu načte.

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Následující obrázek ukazuje počáteční `option1` a `option2` načíst hodnoty z *appSettings.JSON určený* souboru:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Změňte hodnoty *appSettings.JSON určený* do souboru `value1_from_json UPDATED` a `200`. Uložit *appSettings.JSON určený* souboru. Aktualizujte prohlížeč a že jsou aktualizovány hodnoty možnosti:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Možnosti podpory s IConfigureNamedOptions s názvem

S názvem podporu možnosti s [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) je znázorněn příklad &num;6 v [ukázkovou aplikaci](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Vyžaduje základní technologie ASP.NET 2.0 nebo novější.*

*S názvem možnosti* podpory umožňuje aplikaci k rozlišení mezi pojmenované možnosti konfigurace. V ukázkové aplikace, jsou pojmenované možnosti deklarovat s [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurace](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) metoda:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

Ukázková aplikace používá s názvem možnosti s [IOptionsSnapshot&lt;TOptions&gt;. Získat](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Spuštění ukázkové aplikace, jsou vráceny s názvem možnosti:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`hodnoty jsou uvedeny z konfigurace, které jsou načteny z *appSettings.JSON určený* souboru. `named_options_2`hodnoty poskytuje:

* `named_options_2` Delegovat v `ConfigureServices` pro `Option1`.
* Výchozí hodnota pro `Option2` poskytované `MyOptions` třídy.

Nakonfigurujte všechny instance s názvem možnosti s [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) metoda. Následující kód konfiguruje `Option1` pro všechny pojmenované instance konfigurace s hodnotou běžné. Přidejte následující kód ručně na `Configure` metoda:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Spuštění ukázkové aplikace po přidání kód vytvoří následující výsledek:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> V technologii ASP.NET Core 2.0 nebo novější jsou všechny možnosti s názvem instance. Existující `IConfigureOption` instance jsou považovány za cílení `Options.DefaultName` instance, kterou je `string.Empty`. `IConfigureNamedOptions`také implementuje `IConfigureOptions`. Výchozí implementaci [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([odkaz na zdroj](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) obsahuje logiku a odpovídajícím způsobem používat. `null` Pojmenované možnost se používá pro všechny pojmenované instance, místo konkrétního pojmenovanou instanci ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) a [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) použít tento názvů).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Vyžaduje základní technologie ASP.NET 2.0 nebo novější.*

Nastavit postconfiguration s [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Postconfiguration spustí po všech [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) konfiguraci:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) je k dispozici po konfigurace s názvem možností:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Použití [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) po nakonfigurovat všechny pojmenované instance konfigurace:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Možnosti objektu pro vytváření, monitorování a mezipaměti

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se používá pro oznámení při `TOptions` instance změnu. `IOptionsMonitor`podporuje možností možnosti, oznámení, změn a `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (jádro ASP.NET 2.0 nebo novější) zodpovídá za vytvoření nové možnosti instancí. Má jeden [vytvořit](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) metoda. Výchozí implementace trvá všech registrovaných `IConfigureOptions` a `IPostConfigureOptions` a spustí všechny první nakonfiguruje, za nímž následuje po konfiguruje. Umožňuje rozlišovat mezi `IConfigureNamedOptions` a `IConfigureOptions` a pouze volá vhodné rozhraní.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (jádro ASP.NET 2.0 nebo novější) používá `IOptionsMonitor` do mezipaměti `TOptions` instance. `IOptionsMonitorCache` By způsobila neplatnost instance možnosti v monitorování tak, aby hodnota je přepočítávány ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Hodnoty mohou být ručně zavedené i [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). [Zrušte](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) metoda se používá, když všechny pojmenované instance by měl být znovu vytvořen na vyžádání.

## <a name="accessing-options-during-startup"></a>Přístup k možnosti při spuštění

`IOptions`mohou být používány `Configure`, protože služby jsou integrované před `Configure` metody. Pokud je součástí zprostředkovatele služeb `ConfigureServices` pro přístup k možnostem, se nebude obsahovat žádné možnosti konfigurace zadané po poskytovatele služeb. Z důvodu řazení registrace služby můžou Proto existovat v nekonzistentní možnosti stavu.

Vzhledem k tomu, že možnosti jsou obvykle načteny z konfigurace, konfigurace mohou být používány spuštění v obou `Configure` a `ConfigureServices`. Příklady použití konfigurace při spuštění, najdete v článku [spuštění aplikace](xref:fundamentals/startup) tématu.

## <a name="see-also"></a>Viz také

* [Konfigurace](xref:fundamentals/configuration/index)
