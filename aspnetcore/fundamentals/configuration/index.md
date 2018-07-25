---
title: Konfigurace v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak použít rozhraní API pro konfiguraci ke konfiguraci aplikace ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228621"
---
# <a name="configuration-in-aspnet-core"></a>Konfigurace v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [označit Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), a [Luke Latham](https://github.com/guardrex)

Konfigurační rozhraní API poskytuje způsob, jak nakonfigurovat v ASP.NET Core webové aplikace založené na seznam dvojic název hodnota. Konfigurace je pro čtení v době běhu z více zdrojů. Dvojice název hodnota mohou být seskupeny do více úrovní hierarchie.

Existují zprostředkovatele konfigurace pro:

* Formáty souborů (INI, JSON a XML).
* Argumenty příkazového řádku.
* Proměnné prostředí.
* Objekty .NET v paměti.
* Nešifrované [manažera tajných](xref:security/app-secrets) úložiště.
* Uložení šifrovaného uživatelského, jako například [Azure Key Vault](xref:security/key-vault-configuration).
* Vlastní zprostředkovatelé (nainstalované nebo vytváření).

Každá hodnota konfigurace mapuje na řetězec klíče. Je dostupná podpora integrované vazbu k deserializaci nastavení do vlastní [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objektu (jednoduchá třída .NET s vlastnostmi).

Možnosti vzor používá k reprezentování skupiny související nastavení možnosti třídy. Další informace o použití vzoru možnosti najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

Příklady uvedené v tomto tématu využívají:

* Nastavení základní cesty aplikace s využitím [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.
* Řešení oddíly konfiguračních souborů s [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.
* Konfigurace vazby s [svázat](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.

Tyto balíčky jsou součástí [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Příklady uvedené v tomto tématu využívají:

* Nastavení základní cesty aplikace s využitím [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.
* Řešení oddíly konfiguračních souborů s [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.
* Konfigurace vazby s [svázat](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.

Tyto balíčky jsou součástí [metabalíček Microsoft.aspnetcore.all](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Příklady uvedené v tomto tématu využívají:

* Nastavení základní cesty aplikace s využitím [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) balíčku.
* Řešení oddíly konfiguračních souborů s [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) balíčku.
* Konfigurace vazby s [svázat](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` je k dispozici pro aplikaci odkazováním [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) balíčku.

::: moniker-end

## <a name="json-configuration"></a>Konfigurace JSON

Následující aplikace konzoly používá zprostředkovatel konfigurace JSON:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

Aplikace načte a zobrazí následující nastavení:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

Konfigurace se skládá z hierarchický seznam dvojic název hodnota, ve kterých uzlech jsou odděleny dvojtečkou (`:`). Pokud chcete načíst hodnotu, přístup k `Configuration` indexer s klíčem odpovídající položka:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

Pro práci s poli v konfiguraci ve formátu JSON zdrojích, použijte jako součást řetězců oddělených indexu pole. Následující příklad získá název první položky v předchozím `wizards` pole:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Dvojice název hodnota zapsána do integrovaného [konfigurace](/dotnet/api/microsoft.extensions.configuration) zprostředkovatelé jsou **není** trvalé. Ale je možné vytvořit vlastní poskytovatele, který uloží hodnoty. Zobrazit [vlastního poskytovatele konfigurace](xref:fundamentals/configuration/index#custom-config-providers).

V předchozím příkladu používá ke čtení hodnot konfigurace indexeru. Získat přístup ke konfiguraci mimo `Startup`, použijte *možnosti vzor*. Další informace najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.

## <a name="xml-configuration"></a>Konfiguraci XML

Pro práci s poli v konfiguraci ve formátu XML zdroje poskytují `name` index na každý prvek. Použití indexu pro přístup k hodnotám:

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>Konfigurace podle prostředí

Je obvykle mají různá nastavení konfigurace pro různá prostředí, třeba vývoj, testování a produkce. `CreateDefaultBuilder` Metody rozšíření v aplikaci ASP.NET Core 2.x (nebo pomocí `AddJsonFile` a `AddEnvironmentVariables` přímo v aplikaci ASP.NET Core 1.x) přidá poskytovatele konfigurace pro čtení souborů JSON a systém konfigurace zdroje:

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* Proměnné prostředí

Aplikace ASP.NET Core 1.x potřebné k volání `AddJsonFile` a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Zobrazit [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) vysvětlení parametrů. `reloadOnChange` je podporován pouze v ASP.NET Core 1.1 nebo novější.

Konfigurace zdrojů se čtení v pořadí, zda jste zadali. V předchozím kódu jsou proměnné prostředí načteny poslední. Všechny hodnoty konfigurace nastavit pomocí prostředí nahradit v dva poskytovatelé předchozí nastavení.

Vezměte v úvahu následující *appsettings. Staging.JSON* souboru:

[!code-json[](index/sample/appsettings.Staging.json)]

V následujícím kódu `Configure` přečte hodnotu `MyConfig`:

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

Prostředí se obvykle nastavuje na `Development`, `Staging`, nebo `Production`. Další informace najdete v tématu [používání více prostředí](xref:fundamentals/environments).

Požadavky na konfiguraci:

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) znovu načíst konfigurační data při změně.
* Konfigurační klíče jsou **není** malá a velká písmena.
* **Nikdy** ukládat hesla a jiná citlivá data v konfiguraci zprostředkovatele kódu nebo v konfiguračních souborech na prostý text. Nechcete používat produkční tajných kódů při vývoji nebo testovací prostředí. Zadejte tajných kódů mimo projekt tak, že nemohou být omylem zaměřuje na úložiště zdrojového kódu. Další informace o [jak používat více prostředí](xref:fundamentals/environments) a správu [bezpečné ukládání tajných kódů aplikace při vývoji](xref:security/app-secrets).
* Hierarchické konfigurační hodnoty zadané v proměnné prostředí, dvojtečkou (`:`) nemusí fungovat na všech platformách. Dvojitým podtržítkem (`__`) podporuje všechny platformy.
* Při interakci s konfigurací rozhraní API, dvojtečkou (`:`) funguje na všech platformách.

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Zprostředkovatel v paměti a vytvoření vazby na třídu POCO

Následující příklad ukazuje způsob použití poskytovatele v paměti a vytvořit vazbu na třídu:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Hodnoty konfigurace se vrátí jako řetězce, ale vazba umožňuje konstrukce objektů. Vazba umožňuje načtení objektů POCO nebo dokonce celý objekt grafu.

### <a name="getvalue"></a>GetValue

Následující příklad ukazuje, [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) – metoda rozšíření:

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder `GetValue<T>` metoda umožňuje specifikace výchozí hodnotu (80 v ukázce). `GetValue<T>` bez vazby na celé části je pro jednoduché scénáře. `GetValue<T>` Získá skalárních hodnot z `GetSection(key).Value` převedeny na určitý typ.

## <a name="bind-to-an-object-graph"></a>Vytvořit vazbu grafu objektu

Každý objekt ve třídě může být vázaný rekurzivně. Vezměte v úvahu následující `AppSettings` třídy:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

Následující příklad vytvoří vazbu `AppSettings` třídy:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** a vyšší můžete použít `Get<T>`, který pracuje s celé oddíly. `Get<T>` může být výhodnější než použití `Bind`. Následující kód ukazuje, jak používat `Get<T>` s předchozím příkladu:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Pomocí následujících *appsettings.json* souboru:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

Program zobrazí `Height 11`.

Následující kód slouží k jednotce pro test konfigurace:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Vytvoření vlastního zprostředkovatele Entity Framework

V této části se vytvoří zprostředkovatel základní konfigurace, který přečte dvojice název hodnota v databázi pomocí EF.

Definování `ConfigurationValue` entity pro ukládání hodnoty konfigurace v databázi:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Přidat `ConfigurationContext` ukládání a přístup k nakonfigurované hodnoty:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Vytvořte třídu, která implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Vytvoření vlastního poskytovatele konfigurace děděním z [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Poskytovatel konfigurace inicializuje databáze, pokud je prázdný:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Zvýrazněné hodnoty z databáze ("value_from_ef_1" a "value_from_ef_2") se zobrazí při spuštění ukázky.

`EFConfigSource` Lze použít rozšiřující metodu pro přidání zdroj konfigurace:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Následující kód ukazuje, jak použít vlastní `EFConfigProvider`:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Poznámka: Ukázka přidá vlastní `EFConfigProvider` po poskytovatele formátu JSON, takže všechna nastavení z databáze přepíší nastavení z *appsettings.json* souboru.

Pomocí následujících *appsettings.json* souboru:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

Zobrazí se následující výstup:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Zprostředkovatel konfigurace příkazového řádku

[Poskytovatele konfigurace CommandLine](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) přijímá argument příkazového řádku páry klíč hodnota pro konfiguraci za běhu.

[Zobrazení nebo stažení ukázky konfigurace příkazového řádku](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Nastavit a používat ho zprostředkovatel konfigurace příkazového řádku

# <a name="basic-configurationtabbasicconfiguration"></a>[Základní konfigurace](#tab/basicconfiguration/)

Chcete-li aktivovat příkazového řádku konfigurace, zavolejte `AddCommandLine` rozšiřující metody na instanci [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Spuštění kódu se zobrazí následující výstup:

```console
MachineName: MairaPC
Left: 1980
```

Předávání argumentů páry klíč hodnota v příkazovém řádku změní hodnoty `Profile:MachineName` a `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

V okně konzoly se zobrazí:

```console
MachineName: BartPC
Left: 1979
```

Chcete-li přepsat konfiguraci poskytované ostatní poskytovatelé konfigurace pomocí příkazového řádku konfigurace, zavolejte `AddCommandLine` posledního `ConfigurationBuilder`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Typická aplikace ASP.NET Core 2.x použít metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` načte volitelné konfigurace z *appsettings.json*, *appsettings. { Prostředí} .json*, [tajných kódů uživatelů](xref:security/app-secrets) (v `Development` prostředí), proměnné a argumenty příkazového řádku. Zprostředkovatel konfigurace příkazového řádku se nazývá poslední. Poslední volání zprostředkovatele umožňuje argumenty příkazového řádku předané v době běhu k přepsání konfigurace nastavená ostatní poskytovatelé konfigurace volat dříve.

Pro *appsettings* soubory, kde:

* `reloadOnChange` je povolená.
* Obsahují stejné nastavení argumentů příkazového řádku a *appsettings* souboru.
* *Appsettings* změní soubor, který obsahuje odpovídající argument příkazového řádku po spuštění aplikace.

Pokud jsou splněné všechny výše uvedené podmínky, argumenty příkazového řádku se přepíšou.

Můžete použít aplikace ASP.NET Core 2.x [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) místo `CreateDefaultBuilder`. Při použití `WebHostBuilder`, je nutné ručně nastavit konfiguraci s [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Zobrazit na kartě ASP.NET Core 1.x pro další informace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Vytvoření [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) a volat `AddCommandLine` metodu použít poskytovatele konfigurace příkazového řádku. Poslední volání zprostředkovatele umožňuje argumenty příkazového řádku předané v době běhu k přepsání konfigurace nastavená ostatní poskytovatelé konfigurace volat dříve. Použít konfiguraci [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s `UseConfiguration` metody:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Argumenty předané v příkazovém řádku musí odpovídat jednomu ze dvou formátů je znázorněno v následující tabulce:

| Argument formátu                                                     | Příklad        |
| ------------------------------------------------------------------- | :------------: |
| Jeden argument: pár klíč hodnota oddělené symbolem rovná (`=`) | `key1=value`   |
| Posloupnost dva argumenty: pár klíč hodnota oddělené mezerou    | `/key1 value1` |

**Jediný argument**

Hodnota musí následovat znak rovná se (`=`). Hodnota může mít hodnotu null (například `mykey=`).

Klíč může mít předponu.

| Klíčová předpona               | Příklad         |
| ------------------------ | :-------------: |
| Žádná předpona.                | `key1=value1`   |
| Jeden pomlčka (`-`)&#8224; | `-key2=value2`  |
| Dvě pomlčky (`--`)        | `--key3=value3` |
| Lomítko (`/`)      | `/key4=value4`  |

&#8224;Klíč s předponou jediného (`-`) musí být zadaná ve [přepnout mapování](#switch-mappings), které jsou popsány níže.

Příklad příkazu:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Poznámka: Pokud `-key2` není k dispozici v [přepnout mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.

**Pořadí dvou argumentů**

Hodnota nemůže mít hodnotu null a musí dodržovat klíč oddělené mezerou.

Klíč musí mít předponu.

| Klíčová předpona               | Příklad         |
| ------------------------ | :-------------: |
| Jeden pomlčka (`-`)&#8224; | `-key1 value1`  |
| Dvě pomlčky (`--`)        | `--key2 value2` |
| Lomítko (`/`)      | `/key3 value3`  |

&#8224;Klíč s předponou jediného (`-`) musí být zadaná ve [přepnout mapování](#switch-mappings), které jsou popsány níže.

Příklad příkazu:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Poznámka: Pokud `-key1` není k dispozici v [přepnout mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.

### <a name="duplicate-keys"></a>Duplicitní klíče

Pokud duplicitní klíče jsou k dispozici, použije se poslední páru klíč hodnota.

### <a name="switch-mappings"></a>Přepnout mapování

Při ruční sestavení konfigurace s `ConfigurationBuilder`, slovník mapování přepínače lze přidat do `AddCommandLine` metody. Mapování přepínači povolit název klíče pro náhradní logiku.

Při použití přepínače slovníku mapování slovníku se kontroluje u klíč, který odpovídá key určeného tímto argumentem příkazového řádku. Pokud je nalezen příkazového řádku klíč ve slovníku, hodnota slovníku (náhradní klíč) se předá zpět do nastavení konfigurace. Mapování přepínač je vyžadován pro libovolného příkazového řádku klíče předponu jeden pomlčka (`-`).

Přepnout mapování slovníku klíčových pravidel:

* Přepínače musí začínat pomlčkou (`-`) nebo dvojitou pomlčka (`--`).
* Slovník mapování přepínač nesmí obsahovat duplicitní klíče.

V následujícím příkladu `GetSwitchMappings` metoda umožňuje používat jeden dash argumenty příkazového řádku (`-`) předpona klíče a vyhnout se počáteční podklíče předpony.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Bez zadání argumentů příkazového řádku, které slovník `AddInMemoryCollection` nastaví hodnoty konfigurace. Spusťte aplikaci následujícím příkazem:

```console
dotnet run
```

V okně konzoly se zobrazí:

```console
MachineName: RickPC
Left: 1980
```

Použijte následující postupy k předávání nastavení konfigurace:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

V okně konzoly se zobrazí:

```console
MachineName: DahliaPC
Left: 1984
```

Po vytvoření slovníku mapování přepínač obsahuje data zobrazená v následující tabulce:

| Key            | Hodnota                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Abychom si předvedli klíče přepínání pomocí slovníku, spusťte následující příkaz:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Jsou přehozeny příkazového řádku klíče. V okně konzoly se zobrazí konfigurační hodnoty pro `Profile:MachineName` a `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>soubor Web.config

A *web.config* soubor je vyžadován při hostování aplikace v IIS nebo IIS Express. Nastavení v *web.config* povolit [modul ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) spusťte aplikaci a nakonfigurovat další nastavení služby IIS a modulů. Pokud *web.config* není k dispozici soubor a soubor projektu obsahuje `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikování projektu vytvoří *web.config* souborů v publikované výstupu ( *publikovat* složky). Další informace najdete v tématu [hostitele ASP.NET Core ve Windows se službou IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="access-configuration-during-startup"></a>Konfigurace přístupu při spuštění

Získat přístup ke konfiguraci v rámci `ConfigureServices` nebo `Configure` během spuštění, podívejte se na příklady v [spuštění aplikace](xref:fundamentals/startup) tématu.

## <a name="adding-configuration-from-an-external-assembly"></a>Přidání konfigurace z externího sestavení

[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementace umožňuje přidání vylepšení do aplikace při spuštění z externího sestavení mimo aplikaci prvku `Startup` třídy. Další informace najdete v tématu [vylepšení aplikace z externího sestavení](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Konfigurace přístupu v zobrazení stránky Razor nebo MVC

Chcete-li získat přístup k nastavení konfigurace v zobrazení MVC nebo stránky Razor Pages, přidejte [using – direktiva](xref:mvc/views/razor#using) ([referenční dokumentace jazyka C#: použití direktivy](/dotnet/csharp/language-reference/keywords/using-directive)) pro [Microsoft.Extensions.Configuration obor názvů ](/dotnet/api/microsoft.extensions.configuration) a vložit [parametry IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) do stránky nebo zobrazení.

Na stránce pro stránky Razor:

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

V zobrazení MVC:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a>Další poznámky

* Dependency Injection (DI) není nastavená nahoru, až po `ConfigureServices` je vyvolána.
* Konfigurace systému není DI vědět.
* `IConfiguration` má dvě specializace:
  * `IConfigurationRoot` Používá se pro kořenový uzel. Můžete aktivovat znova načíst.
  * `IConfigurationSection` Reprezentuje oddíl konfigurační hodnoty. `GetSection` a `GetChildren` metody vrátit `IConfigurationSection`.
  * Použití [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) při opětovném načtení konfigurace nebo pro přístup do každého zprostředkovatele. Ani jeden z těchto situacích jsou běžné.

## <a name="additional-resources"></a>Další zdroje

* [Možnosti](xref:fundamentals/configuration/options)
* [Používání více prostředí](xref:fundamentals/environments)
* [Bezpečné ukládání tajných kódů aplikace při vývoji](xref:security/app-secrets)
* [Hostitel v ASP.NET Core](xref:fundamentals/host/index)
* [Injektáž závislostí](xref:fundamentals/dependency-injection)
* [Zprostředkovatel konfigurace služby Azure Key Vault](xref:security/key-vault-configuration)
