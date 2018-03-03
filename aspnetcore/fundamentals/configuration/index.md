---
title: Konfigurace v ASP.NET Core
author: rick-anderson
description: "Použijte rozhraní API konfigurace pro konfiguraci aplikace ASP.NET Core několik metod."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 8f52f2dc9515761510de870f10ad0975401db74a
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="configure-an-aspnet-core-app"></a>Konfigurace aplikace ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [označit Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [ADAM Roth](https://github.com/danroth27), a [Luke Latham](https://github.com/guardrex)

Rozhraní API konfigurace poskytuje způsob, jak nakonfigurovat ASP.NET Core webové aplikace založené na seznam dvojic název hodnota. Konfigurace je pro čtení, v době běhu z více zdrojů. Dvojice název hodnota, je možné seskupit do víceúrovňovou hierarchii.

Existují zprostředkovatelé konfigurace pro:

* Formáty souborů (INI, JSON a XML)
* Argumenty příkazového řádku
* Proměnné prostředí
* Objekty .NET v paměti
* Úložišti šifrované uživatele
* [Azure Key Vault](xref:security/key-vault-configuration)
* Vlastní zprostředkovatelé (nainstalována nebo vytvořili)

Každá hodnota konfigurace se mapuje na řetězec klíč. Je dostupná podpora předdefinované vazby k deserializaci nastavení do vlastní [objektů POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objektu (jednoduché rozhraní .NET třídu s vlastnostmi).

Vzor možnosti používá k reprezentování skupiny související nastavení možnosti třídy. Další informace o použití vzoru možnosti najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>Konfigurace JSON

Následující konzolové aplikace používá zprostředkovatele konfigurace JSON:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

Aplikace načte a zobrazí následující nastavení:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

Konfigurace se skládá z hierarchický seznam dvojic název hodnota, ve kterých jsou uzly oddělené dvojtečkou. Pokud chcete načíst hodnotu, přístup k `Configuration` indexer klíčem odpovídající položky:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

Pro práci s pole ve formátu JSON konfigurace zdrojů, použijte pole indexu jako součást řetězce oddělené dvojtečkou. Následující příklad načte název první položky v předchozím `wizards` pole:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Dvojice název hodnota, které jsou zapsány do vestavěné [konfigurace](/dotnet/api/microsoft.extensions.configuration) poskytovatelé jsou **není** nastavené jako trvalé. Však lze vytvořit vlastní zprostředkovatele, který uloží hodnoty. V tématu [vlastního poskytovatele konfigurace](xref:fundamentals/configuration/index#custom-config-providers).

V předchozím příkladu používá konfigurace indexeru načíst hodnoty. Získat přístup ke konfiguraci mimo `Startup`, použijte *možnosti vzor*. Další informace najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.

## <a name="xml-configuration"></a>Konfigurační soubor XML

Chcete-li pracovat s pole ve formátu XML konfigurace zdrojů, zadat `name` index pro každý prvek. Index použijte pro přístup k hodnoty:

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

## <a name="configuration-by-environment"></a>Konfigurace prostředí

Je typické jinou konfiguraci nastavení pro různá prostředí, například vývoj, testování a provozním. `CreateDefaultBuilder` v aplikaci ASP.NET Core 2.x – metoda rozšíření (nebo pomocí `AddJsonFile` a `AddEnvironmentVariables` přímo v aplikaci ASP.NET Core 1.x) přidá zprostředkovatele konfigurace pro čtení soubory JSON a systému konfigurace zdrojů:

* *appsettings.json*
* *appsettings.\<EnvironmentName>.json*
* Proměnné prostředí

Aplikace ASP.NET Core 1.x muset volat `AddJsonFile` a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

V tématu [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) vysvětlení parametrů. `reloadOnChange` je podporován pouze v ASP.NET Core 1.1 nebo novější.

Konfigurace zdroje se čtou v pořadí, zda jste zadali. V předchozím kódu se čtou poslední proměnné prostředí. Všechny hodnoty konfigurace nastavit prostřednictvím prostředí nahradit nastavené v dva poskytovatelé předchozí.

Vezměte v úvahu následující *appsettings. Staging.JSON* souboru:

[!code-json[](index/sample/appsettings.Staging.json)]

Pokud se nastaví prostředí `Staging`, následující `Configure` metoda přečte hodnotu `MyConfig`:

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


V prostředí se obvykle nastavuje na `Development`, `Staging`, nebo `Production`. Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).

Požadavky na konfiguraci:

* `IOptionsSnapshot` můžete znovu načíst konfigurační data, kdy se změní. Další informace najdete v tématu [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,
* Konfigurace klíče jsou **není** malá a velká písmena.
* **Nikdy** ukládání hesel nebo jiných citlivých dat. kód zprostředkovatele konfigurace nebo v konfiguračních souborech na prostý text. Nechcete používat produkční tajných klíčů v vývoj nebo testovací prostředí. Zadejte tajné klíče mimo projekt tak, že nemohou být omylem zavazuje úložiště zdrojového kódu. Další informace o [práce s několika prostředí](xref:fundamentals/environments) a správu [bezpečného úložiště tajné klíče aplikace během vývoje](xref:security/app-secrets).
* Pokud dvojtečkou (`:`) nelze použít v seznamu proměnných prostředí v systému, nahraďte dvojtečkou (`:`) s dvojité podtržítko (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Zprostředkovatel v paměti a vazbu na třídu objektů POCO

Následující příklad ukazuje, jak použít poskytovatele v paměti a vytvořte vazbu na třídu:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Hodnoty konfigurace se vrátí jako řetězce, ale vazba umožňuje konstrukce objektů. Vazba umožňuje načtení objektů POCO nebo grafy i celý objekt.

### <a name="getvalue"></a>GetValue

Následující příklad ukazuje [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) metoda rozšíření:

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder `GetValue<T>` metoda umožňuje specifikaci výchozí hodnotu (80 v ukázce). `GetValue<T>` je pro jednoduché scénáře a bez vazby na celý části. `GetValue<T>` Získá skalárních hodnot z `GetSection(key).Value` převést na konkrétního typu.

## <a name="bind-to-an-object-graph"></a>Vytvořit vazbu na grafu objektu

Každý objekt v třídě může být rekurzivně vázána. Vezměte v úvahu následující `AppSettings` třídy:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

Následující příklad vytvoří vazbu `AppSettings` třídy:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** a vyšší můžete použít `Get<T>`, který pracuje s celé oddíly. `Get<T>` může být vhodnější než použití `Bind`. Následující kód ukazuje, jak používat `Get<T>` s v předchozím příkladu:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Pomocí následujících *appSettings.JSON určený* souboru:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

Program zobrazí `Height 11`.

Následující kód slouží k jednotce otestovat konfiguraci:

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

V této části se vytvoří základní konfiguraci poskytovatele, který čte dvojice název hodnota v databázi pomocí EF.

Definování `ConfigurationValue` entity pro ukládání hodnoty konfigurace v databázi:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Přidat `ConfigurationContext` ukládání a přístup k nakonfigurované hodnoty:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Vytvořte třídu, která implementuje [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Vytvoření vlastního poskytovatele konfigurace dědění ze [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Poskytovatel konfigurace inicializuje databázi, pokud je prázdné:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Při spuštění ukázky, zobrazí se zvýrazněné hodnoty z databáze ("value_from_ef_1" a "value_from_ef_2").

`EFConfigSource` Rozšíření metodu pro přidání zdroj konfigurace je možné použít:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Následující kód ukazuje, jak používat vlastní `EFConfigProvider`:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Poznámka: Ukázka přidá vlastní `EFConfigProvider` po poskytovatele JSON, takže všechna nastavení z databáze přepíší nastavení z *appSettings.JSON určený* souboru.

Pomocí následujících *appSettings.JSON určený* souboru:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

Zobrazí se následující výstup:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Poskytovatel konfigurace příkazového řádku

[Poskytovatele konfigurace CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) obdrží páry klíč hodnota argument příkazového řádku pro konfiguraci za běhu.

[Zobrazení nebo stažení ukázky konfigurace příkazového řádku](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Instalační program a používají zprostředkovatele konfigurace příkazového řádku

# <a name="basic-configurationtabbasicconfiguration"></a>[Základní konfigurace](#tab/basicconfiguration)

Chcete-li aktivovat konfigurace příkazového řádku, zavolejte `AddCommandLine` rozšiřující metody na instanci [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Spuštění kódu, zobrazí se následující výstup:

```console
MachineName: MairaPC
Left: 1980
```

Předání argumentů páry klíč hodnota na příkazovém řádku změní hodnoty `Profile:MachineName` a `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

V okně konzoly zobrazí:

```console
MachineName: BartPC
Left: 1979
```

Chcete-li přepsat konfiguraci poskytované jiných poskytovatelů konfigurace s konfigurací příkazového řádku, volejte `AddCommandLine` poslední na `ConfigurationBuilder`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Typická aplikace ASP.NET Core 2.x použít metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` načte volitelné konfiguraci z *appSettings.JSON určený*, *appsettings. { Prostředí} .json*, [tajné klíče uživatele](xref:security/app-secrets) (v `Development` prostředí), proměnné prostředí a argumenty příkazového řádku. Poskytovatel konfigurace příkazového řádku se označuje jako poslední. Poslední volání zprostředkovatele umožňuje dříve názvem argumenty příkazového řádku předaný běhu přepsat konfiguraci nastavit pomocí jiných poskytovatelů konfigurace.

Pro *appsettings* soubory kde:

* `reloadOnChange` je povolené.
* Obsahovat stejnému nastavení v argumenty příkazového řádku a *appsettings* souboru.
* *Appsettings* souboru, který obsahuje odpovídající argument příkazového řádku je změnit po spuštění aplikace.

Pokud jsou splněny všechny předchozí podmínky, se přepíšou argumenty příkazového řádku.

Můžete použít aplikaci ASP.NET Core 2.x [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) místo `CreateDefaultBuilder`. Při použití `WebHostBuilder`, je nutné ručně nastavit konfiguraci s [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). V tématu kartě ASP.NET Core 1.x pro další informace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Vytvoření [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) a volání `AddCommandLine` metodu použít poskytovatele konfigurace příkazového řádku. Poslední volání zprostředkovatele umožňuje dříve názvem argumenty příkazového řádku předaný běhu přepsat konfiguraci nastavit pomocí jiných poskytovatelů konfigurace. Použít konfiguraci [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s `UseConfiguration` metoda:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Argumenty předávané na příkazovém řádku musí odpovídat jednomu ze dvou formátů uvedené v následující tabulce:

| Argument formátu                                                     | Příklad        |
| ------------------------------------------------------------------- | :------------: |
| Jeden argument: pár klíč hodnota oddělená symbolem rovná se (`=`) | `key1=value`   |
| Pořadí dva argumenty: pár klíč hodnota oddělené mezerou    | `/key1 value1` |

**Jeden argument**

Hodnota musí následovat znak rovná se (`=`). Hodnota může mít hodnotu null (například `mykey=`).

Klíč může mít předponu.

| Předpona klíče               | Příklad         |
| ------------------------ | :-------------: |
| Žádná předpona.                | `key1=value1`   |
| Jednotné dash (`-`) &#8224; | `-key2=value2`  |
| Dvě pomlčky (`--`)        | `--key3=value3` |
| Lomítko (`/`)      | `/key4=value4`  |

&#8224; Klíč s předponou jediného (`-`) musí být zadáno v [přepínač mapování](#switch-mappings), které jsou popsány níže.

Příklad:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Poznámka: Pokud `-key1` není součástí [přepínač mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.

**Pořadí dva argumenty**

Hodnota nesmí být null a musí následovat klíč oddělené mezerou.

Klíč musí mít předponu.

| Předpona klíče               | Příklad         |
| ------------------------ | :-------------: |
| Jednotné dash (`-`) &#8224; | `-key1 value1`  |
| Dvě pomlčky (`--`)        | `--key2 value2` |
| Lomítko (`/`)      | `/key3 value3`  |

&#8224; Klíč s předponou jediného (`-`) musí být zadáno v [přepínač mapování](#switch-mappings), které jsou popsány níže.

Příklad:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Poznámka: Pokud `-key1` není součástí [přepínač mapování](#switch-mappings) předaná zprostředkovateli konfigurace `FormatException` je vyvolána výjimka.

### <a name="duplicate-keys"></a>Duplicitní klíče

Pokud duplicitní klíče jsou k dispozici, použije se poslední dvojice klíč hodnota.

### <a name="switch-mappings"></a>Mapování přepínače

Při ruční vytváření konfigurací s `ConfigurationBuilder`, slovník mapování přepínače lze přidat do `AddCommandLine` metoda. Mapování přepínač Povolit logiku nahrazení název klíče.

Když se používá slovník mapování přepínače, se kontroluje slovníku pro klíč, který se shoduje s klíčem poskytované argument příkazového řádku. Pokud je nalezen příkazového řádku klíč ve slovníku, hodnota slovníku (klíče nahrazení) je předán zpět k nastavení konfigurace. Mapování přepínač je vyžadována pro všechny klíč příkazového řádku s předponou jeden pomlčkou (`-`).

Přepínač pravidla klíče slovníku mapování:

* Přepínače musí začínat pomlčkou (`-`) nebo dvojitou pomlčkou (`--`).
* Slovník mapování přepínač nesmí obsahovat duplicitní klíče.

V následujícím příkladu `GetSwitchMappings` metoda umožňuje argumentů příkazového řádku pro použití jedné pomlčkou (`-`) klíče předponu a vyhnout se počáteční podklíčů předpony.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Bez zadání argumentů příkazového řádku, slovníku poskytované `AddInMemoryCollection` nastaví hodnoty konfigurace. Spusťte aplikaci pomocí následujícího příkazu:

```console
dotnet run
```

V okně konzoly zobrazí:

```console
MachineName: RickPC
Left: 1980
```

Použijte následující postupy k předávání v nastavení konfigurace:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

V okně konzoly zobrazí:

```console
MachineName: DahliaPC
Left: 1984
```

Po vytvoření slovníku mapování přepínač obsahuje data zobrazená v následující tabulce:

| Key            | Hodnota                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

K předvedení klíče přepínání pomocí slovníku, spusťte následující příkaz:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Příkazového řádku klíče jsou vzájemně zaměněny. V okně konzoly zobrazí hodnoty konfigurace pro `Profile:MachineName` a `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>web.config file

A *web.config* soubor je požadován při hostování aplikace v IIS nebo IIS Express. Nastavení v *web.config* povolit [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) spusťte aplikaci a nakonfigurovat další nastavení služby IIS a modulů. Pokud *web.config* soubor není přítomen a zahrnuje soubor projektu `<Project Sdk="Microsoft.NET.Sdk.Web">`, publikování projektu vytvoří *web.config* souboru v publikované výstup ( *publikování* složku). Další informace najdete v tématu [hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="accessing-configuration-during-startup"></a>Přístup k konfigurace při spuštění

Získat přístup ke konfiguraci v rámci `ConfigureServices` nebo `Configure` během spouštění, podívejte se na příklady v [spuštění aplikace](xref:fundamentals/startup) tématu.

## <a name="additional-notes"></a>Další poznámky

* Vkládání závislostí (DI) není nastavená až do doby, po `ConfigureServices` je volána.
* Konfigurace systému není DI vědět.
* `IConfiguration` má dva specializací:
  * `IConfigurationRoot` Používá se pro kořenový uzel. Můžete aktivovat znovu načíst.
  * `IConfigurationSection` Reprezentuje oddíl hodnoty konfigurace. `GetSection` a `GetChildren` metody vrací `IConfigurationSection`.
  * Použití [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) při opětovném načtení konfigurace nebo pro přístup do každého poskytovatele. Ani jeden z těchto situacích jsou běžné.

## <a name="additional-resources"></a>Další zdroje

* [Možnosti](xref:fundamentals/configuration/options)
* [Práce s několika prostředí](xref:fundamentals/environments)
* [Bezpečné úložiště tajných částí aplikace při vývoji](xref:security/app-secrets)
* [Hostování v ASP.NET Core](xref:fundamentals/hosting)
* [Injektáž závislostí](xref:fundamentals/dependency-injection)
* [Zprostředkovatel konfigurace služby Azure Key Vault](xref:security/key-vault-configuration)
