---
title: Konfigurace v ASP.NET Core
author: rick-anderson
description: "Použijte rozhraní API konfigurace pro konfiguraci aplikace ASP.NET Core několik metod."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a>Konfigurace aplikace ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [označit Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [ADAM Roth](https://github.com/danroth27), a [Luke Latham](https://github.com/guardrex)

Rozhraní API konfigurace poskytuje způsob, jak nakonfigurovat ASP.NET Core webové aplikace založené na seznam dvojic název hodnota. Konfigurace je pro čtení, v době běhu z více zdrojů. Tyto páry název hodnota můžete seskupovat do víceúrovňovou hierarchii. 

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

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

Aplikace načte a zobrazí následující nastavení:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

Konfigurace se skládá z hierarchický seznam dvojic název hodnota, ve kterých jsou uzly oddělené dvojtečkou. Pokud chcete načíst hodnotu, přístup k `Configuration` indexer klíčem odpovídající položky:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Pro práci s pole ve formátu JSON konfigurace zdrojů, použijte pole indexu jako součást řetězce oddělené dvojtečkou. Následující příklad načte název první položky v předchozím `wizards` pole:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Dvojice název hodnota, které jsou zapsány do vestavěné `Configuration` poskytovatelé jsou **není** nastavené jako trvalé. Můžete však vytvořit vlastní zprostředkovatele, který uloží hodnoty. V tématu [vlastního poskytovatele konfigurace](xref:fundamentals/configuration/index#custom-config-providers).

V předchozím příkladu používá konfigurace indexeru načíst hodnoty. Získat přístup ke konfiguraci mimo `Startup`, použijte *možnosti vzor*. Další informace najdete v tématu [možnosti](xref:fundamentals/configuration/options) tématu.

Je typické jinou konfiguraci nastavení pro různá prostředí, například vývoj, testování a provozním. `CreateDefaultBuilder` v aplikaci ASP.NET Core 2.x – metoda rozšíření (nebo pomocí `AddJsonFile` a `AddEnvironmentVariables` přímo v aplikaci ASP.NET Core 1.x) přidá zprostředkovatele konfigurace pro čtení soubory JSON a systému konfigurace zdrojů:

* *appSettings.JSON určený*
* *appSettings. \<EnvironmentName > .json*
* Proměnné prostředí

V tématu [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) vysvětlení parametrů. `reloadOnChange`je podporován pouze v ASP.NET Core 1.1 nebo novější. 

Konfigurace zdroje se čtou v pořadí, zda jste zadali. Ve výše uvedeném kódu se čtou poslední proměnné prostředí. Všechny hodnoty konfigurace nastavit prostřednictvím prostředí nahradit nastavené v dva poskytovatelé předchozí.

V prostředí se obvykle nastavuje na `Development`, `Staging`, nebo `Production`. V tématu [práce s několika prostředí](xref:fundamentals/environments) Další informace.

Požadavky na konfiguraci:

* `IOptionsSnapshot`můžete znovu načíst konfigurační data, kdy se změní. V tématu [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) Další informace.
* Konfigurace klíče jsou malá a velká písmena.
* Zadejte proměnné prostředí poslední tak, aby místní prostředí můžete přepsat nastavení v nasazené konfigurační soubory.
* **Nikdy** ukládání hesel nebo jiných citlivých dat. kód zprostředkovatele konfigurace nebo v konfiguračních souborech na prostý text. Nechcete používat produkční tajných klíčů ve vývojovém nebo testovacím prostředí. Místo toho zadejte tajné klíče mimo projekt tak, že nemohou být omylem zaměřuje na úložiště. Další informace o [práce s několika prostředí](xref:fundamentals/environments) a správu [bezpečného úložiště tajné klíče aplikace během vývoje](xref:security/app-secrets).
* Pokud dvojtečkou (`:`) nelze použít v seznamu proměnných prostředí systému, nahraďte dvojtečkou (`:`) s dvojité podtržítko (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Zprostředkovatel v paměti a vazbu na třídu objektů POCO

Následující příklad ukazuje, jak použít poskytovatele v paměti a vytvořte vazbu na třídu:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

Hodnoty konfigurace se vrátí jako řetězce, ale vazba umožňuje konstrukce objektů. Vazba umožňuje načíst objektů POCO nebo grafy i celý objekt.

### <a name="getvalue"></a>GetValue

Následující příklad ukazuje [GetValue&lt;T&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) metoda rozšíření:

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder `GetValue<T>` metoda umožňuje zadat výchozí hodnotu (80 v ukázce). `GetValue<T>`je pro jednoduché scénáře a nemá vazbu na celý části. `GetValue<T>`Získá skalárních hodnot z `GetSection(key).Value` převést na konkrétního typu.

## <a name="bind-to-an-object-graph"></a>Vytvořit vazbu na grafu objektu

Můžete rekurzivně vazby pro každý objekt v třídě. Vezměte v úvahu následující `AppSettings` třídy:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

Následující příklad vytvoří vazbu `AppSettings` třídy:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** a vyšší můžete použít `Get<T>`, který pracuje s celé oddíly. `Get<T>`může být vhodnější než použití `Bind`. Následující kód ukazuje, jak používat `Get<T>` s ukázkou výše:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Pomocí následujících *appSettings.JSON určený* souboru:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

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

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Přidat `ConfigurationContext` ukládání a přístup k nakonfigurované hodnoty:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Vytvořte třídu, která implementuje [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Vytvoření vlastního poskytovatele konfigurace dědění ze [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Poskytovatel konfigurace inicializuje databázi, pokud je prázdné:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Při spuštění ukázky, zobrazí se zvýrazněné hodnoty z databáze ("value_from_ef_1" a "value_from_ef_2").

Můžete přidat `EFConfigSource` rozšiřující metodu pro přidání zdroj konfigurace:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Následující kód ukazuje, jak používat vlastní `EFConfigProvider`:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Poznámka: Ukázka přidá vlastní `EFConfigProvider` po poskytovatele JSON, takže všechna nastavení z databáze přepíší nastavení z *appSettings.JSON určený* souboru.

Pomocí následujících *appSettings.JSON určený* souboru:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Zobrazí se:

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

Chcete-li aktivovat konfigurace příkazového řádku, zavolejte `AddCommandLine` rozšiřující metody na instanci [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

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

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Typická aplikace ASP.NET Core 2.x použít metodu statické pohodlí `CreateDefaultBuilder` k sestavení hostitele:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder`načte volitelné konfiguraci z *appSettings.JSON určený*, *appsettings. { Prostředí} .json*, [tajné klíče uživatele](xref:security/app-secrets) (v `Development` prostředí), proměnné prostředí a argumenty příkazového řádku. Poskytovatel konfigurace příkazového řádku se označuje jako poslední. Poslední volání zprostředkovatele umožňuje dříve názvem argumenty příkazového řádku předaný běhu přepsat konfiguraci nastavit pomocí jiných poskytovatelů konfigurace.

Všimněte si, že pro *appsettings* soubory, které `reloadOnChange` je povoleno. Argumenty příkazového řádku jsou přepsat, pokud odpovídající hodnotu konfigurace v *appsettings* dojde ke změně souboru po spuštění aplikace.

> [!NOTE]
> Jako alternativu k použití `CreateDefaultBuilder` metody vytvoření hostitele pomocí [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) a ručně vytváření konfigurací s [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) je podporována v ASP.NET Core 2.x. V tématu kartě ASP.NET Core 1.x pro další informace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Vytvoření [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) a volání `AddCommandLine` metodu použít poskytovatele konfigurace příkazového řádku. Poslední volání zprostředkovatele umožňuje dříve názvem argumenty příkazového řádku předaný běhu přepsat konfiguraci nastavit pomocí jiných poskytovatelů konfigurace. Použít konfiguraci [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) s `UseConfiguration` metoda:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Argumenty předávané na příkazovém řádku musí odpovídat jednomu ze dvou formátů uvedené v následující tabulce.

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

Při ruční vytváření konfigurací s `ConfigurationBuilder`, Volitelně můžete zadat slovníku mapování přepínače k `AddCommandLine` metoda. Mapování přepínače umožňují poskytovat logiku nahrazení název klíče.

Když se používá slovník mapování přepínače, se kontroluje slovníku pro klíč, který se shoduje s klíčem poskytované argument příkazového řádku. Pokud je nalezen příkazového řádku klíč ve slovníku, hodnota slovníku (klíče nahrazení) je předán zpět k nastavení konfigurace. Mapování přepínač je vyžadována pro všechny klíč příkazového řádku s předponou jeden pomlčkou (`-`).

Přepínač pravidla klíče slovníku mapování:

* Přepínače musí začínat pomlčkou (`-`) nebo dvojitou pomlčkou (`--`).
* Slovník mapování přepínač nesmí obsahovat duplicitní klíče.

V následujícím příkladu `GetSwitchMappings` metoda umožňuje vaší argumenty příkazového řádku používat jeden pomlčkou (`-`) klíče předponu a vyhnout se počáteční podklíčů předpony.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

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

Po vytvoření slovníku mapování přepínač obsahuje data zobrazená v následující tabulce.

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

## <a name="the-webconfig-file"></a>V souboru web.config

A *web.config* soubor je povinný, pokud je hostitelem aplikace v IIS a služby IIS Express. *soubor Web.config* zapne AspNetCoreModule ve službě IIS spusťte aplikaci. Nastavení v *web.config* povolit AspNetCoreModule ve službě IIS spusťte aplikaci a nakonfigurovat další nastavení služby IIS a modulů. Pokud používáte Visual Studio a odstranit *web.config*, Visual Studio vytvoří novou.

## <a name="additional-notes"></a>Další poznámky

* Vkládání závislostí (DI) není nastavená až do doby, po `ConfigureServices` je volána.
* Konfigurace systému není DI vědět.
* `IConfiguration`má dva specializací:
  * `IConfigurationRoot`Používá se pro kořenový uzel. Můžete aktivovat znovu načíst.
  * `IConfigurationSection`Reprezentuje oddíl hodnoty konfigurace. `GetSection` a `GetChildren` metody vrací `IConfigurationSection`.

## <a name="additional-resources"></a>Další zdroje

* [Možnosti](xref:fundamentals/configuration/options)
* [Práce s několika prostředí](xref:fundamentals/environments)
* [Bezpečné úložiště tajné klíče aplikace během vývoje](xref:security/app-secrets)
* [Hostování v ASP.NET Core](xref:fundamentals/hosting)
* [Vkládání závislostí](xref:fundamentals/dependency-injection)
* [Poskytovatel konfigurace služby Azure Key Vault](xref:security/key-vault-configuration)
