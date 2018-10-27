---
uid: config-builder
title: Konfigurace počítačů pro technologii ASP.NET
author: rick-anderson
description: Další informace o získání konfigurační data ze zdrojů než hodnoty web.config z externích zdrojů.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148834"
---
# <a name="configuration-builders-for-aspnet"></a>Konfigurace počítačů pro technologii ASP.NET

Podle [Stephen Molloy](https://github.com/StephenMolloy) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Konfigurace počítačů poskytují moderní a agilní mechanismus pro aplikace v ASP.NET k získání hodnoty konfigurace z externích zdrojů.

Konfigurace počítačů:

* Jsou k dispozici v rozhraní .NET Framework 4.7.1 a novějším.
* Nabízejí flexibilní mechanismus pro čtení konfigurační hodnoty.
* Řešení některých základních potřeb aplikace tak, jak přesunout do kontejneru a fokusem prostředí v cloudu.
* Můžete použít ke zlepšení ochrany konfigurační data kreslením z dříve k dispozici zdrojů (například Azure Key Vault a proměnnými prostředí) v konfigurační systém .NET.

## <a name="keyvalue-configuration-builders"></a>Tvůrci konfigurace klíč hodnota

Běžný scénář, který může být zpracována tvůrci konfigurace je mechanismus nahrazení základní klíč/hodnota pro konfigurační oddíly funkce, které se řídí vzorem klíč/hodnota. Rozhraní .NET Framework konceptu ConfigurationBuilders není omezena pouze na konkrétní konfigurační oddíly funkce nebo vzorce. Nicméně mnoho tvůrci konfigurace v `Microsoft.Configuration.ConfigurationBuilders` ([githubu](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) práce v rámci vzoru klíč/hodnota.

## <a name="keyvalue-configuration-builders-settings"></a>Nastavení tvůrci konfigurace klíč hodnota

Následující nastavení se vztahuje na všechny konfigurace Tvůrce klíč/hodnota v `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Režim

Tvůrci konfiguraci pomocí externího zdroje informací klíč/hodnota k naplnění vybraný klíč/hodnota prvků konfigurace systému. Konkrétně `<appSettings/>` a `<connectionStrings/>` oddíly přijímat zvláštní zacházení z konfigurace počítačů. Tvůrci pracovat v tří režimů:

* `Strict` – Výchozí režim. V tomto režimu konfigurace Tvůrce pracuje pouze se dobře známé klíč/hodnota-zaměřenou na konfigurační oddíly funkce. `Strict` Režim zobrazí každý klíč v části. Pokud se najde odpovídající klíče v externím zdroji:

   * Tvůrci konfigurace nahraďte hodnotu v konfigurační sekci výslednou hodnotu z externího zdroje.
* `Greedy` – Tento režim je úzce souvisí s `Strict` režimu. Místo omezeno na klíče, které již existují v původní konfiguraci:

  * Konfigurace počítačů přidá všechny páry klíč/hodnota z externího zdroje do výsledné konfigurační oddíl.

* `Expand` -Funguje v nezpracovaném kódu XML předtím, než je analyzován do konfiguračního oddílu objektu. To můžete představit jako rozšíření tokenů v řetězci. Libovolná součást Nezpracovaný řetězec XML, který odpovídá vzoru `${token}` je kandidát pro rozšíření tokenu. Pokud není nalezena žádná odpovídající hodnota v externím zdroji, token, který se nemění. Tvůrci v tomto režimu nejsou omezeny `<appSettings/>` a `<connectionStrings/>` oddíly.

Následující kód z *web.config* umožňuje [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) v `Strict` režimu:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Následující kód čtení `<appSettings/>` a `<connectionStrings/>` je znázorněno v předchozím *web.config* souboru:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Předchozí kód nastaví hodnoty vlastností:

* Hodnoty *web.config* souboru, pokud nejsou nastavené klíče v seznamu proměnných prostředí.
* Hodnoty prostředí proměnné, když nastavení.

Například `ServiceID` bude obsahovat:

* "ServiceID hodnotu ze souboru web.config", pokud je proměnná prostředí `ServiceID` není nastaven.
* Hodnota `ServiceID` prostředí proměnné, když nastavení.

Na následujícím obrázku `<appSettings/>` klíče/hodnoty z předchozího *web.config* souboru sadu v editoru prostředí:

![editor prostředí](config-builder/static/env.png)

Poznámka: Můžete potřebovat ukončit a restartovat Visual Studio pro zobrazení změn v proměnných prostředí.

### <a name="prefix-handling"></a>Předpona zpracování

Předpony klíčů můžete zjednodušit nastavení klíče, protože:

* Konfigurace rozhraní .NET Framework je složitá a vnořené.
* Externí klíč/hodnota zdroje jsou často na úrovni basic a bez stromové struktury podle povahy. Například nejsou vnořené proměnné prostředí.

Použít některou z následujících dvou přístupů vkládat obě `<appSettings/>` a `<connectionStrings/>` do konfigurace prostřednictvím proměnné prostředí:

* S `EnvironmentConfigBuilder` ve výchozím `Strict` režimu a odpovídající názvy klíčů v konfiguračním souboru. Předchozí kód a kód používá tento přístup. Tento přístup můžete **není** stejně jako s názvem klíče v obou `<appSettings/>` a `<connectionStrings/>`.
* Použijte dva `EnvironmentConfigBuilder`ve `Greedy` režimu s odlišné předpony a `stripPrefix`. Díky tomuto přístupu může číst aplikace `<appSettings/>` a `<connectionStrings/>` bez nutnosti aktualizovat konfigurační soubor. Další části [stripPrefix](#stripprefix), ukazuje, jak to provést.
* Použijte dva `EnvironmentConfigBuilder`ve `Greedy` režimu s odlišné předpony. Díky tomuto přístupu nemůže mít duplicitní názvy klíčů jako názvy klíčů se musí lišit podle předpony.  Příklad:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

S předchozím kódu je možné zadat konfiguraci pro dvě různé oddíly stejného zdroje bez stromové struktury klíč/hodnota.

Na následujícím obrázku `<appSettings/>` a `<connectionStrings/>` klíče/hodnoty z předchozího *web.config* souboru sadu v editoru prostředí:

![editor prostředí](config-builder/static/prefix.png)

Následující kód čtení `<appSettings/>` a `<connectionStrings/>` klíče/hodnoty obsažené v předchozím *web.config* souboru:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Předchozí kód nastaví hodnoty vlastností:

* Hodnoty *web.config* souboru, pokud nejsou nastavené klíče v seznamu proměnných prostředí.
* Hodnoty prostředí proměnné, když nastavení.

Například použití předchozího *web.config* souboru, jsou nastavené klíče/hodnoty v předchozím obrázku editor prostředí a předchozí kód, následující hodnoty:

|  Key              | Hodnota |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID Obálka proměnných|
|    AppSetting_default            | AppSetting_default hodnotu z prostředí |
|       ConnStr_default         | Val ConnStr_default z env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: logickou hodnotu, výchozí hodnota je `false`. 

Předchozí kód XML odděluje nastavení aplikace připojovací řetězce, ale vyžaduje všechny klíče v *web.config* souboru pomocí zadané předpony. Například předponu `AppSetting` musí být přidané do `ServiceID` klíč ("AppSetting_ServiceID"). S `stripPrefix`, předpona, která se nepoužívá *web.config* souboru. Předpona, která se vyžaduje Tvůrce zdroj konfigurace (například v prostředí.) Předpokládáme, že se Většina vývojářů používá `stripPrefix`.

Aplikace obvykle pruhu vypnout předponu. Následující *web.config* odstraní předpona:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

V předchozím *web.config* souboru, `default` klíč je v obou `<appSettings/>` a `<connectionStrings/>`.

Na následujícím obrázku `<appSettings/>` a `<connectionStrings/>` klíče/hodnoty z předchozího *web.config* souboru sadu v editoru prostředí:

![editor prostředí](config-builder/static/prefix.png)

Následující kód čtení `<appSettings/>` a `<connectionStrings/>` klíče/hodnoty obsažené v předchozím *web.config* souboru:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Předchozí kód nastaví hodnoty vlastností:

* Hodnoty *web.config* souboru, pokud nejsou nastavené klíče v seznamu proměnných prostředí.
* Hodnoty prostředí proměnné, když nastavení.

Například použití předchozího *web.config* souboru, jsou nastavené klíče/hodnoty v předchozím obrázku editor prostředí a předchozí kód, následující hodnoty:

|  Key              | Hodnota |
| ----------------- | ------------ |
|     ID služby           | AppSetting_ServiceID Obálka proměnných|
|    default            | AppSetting_default hodnotu z prostředí |
|    default         | Val ConnStr_default z env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Řetězec, výchozí hodnota je `@"\$\{(\w+)\}"`

`Expand` Chování tvůrci vyhledá nezpracovaném kódu XML pro tokeny, které vypadají jako `${token}`. Vyhledávání se provádí s regulárním výrazem výchozí `@"\$\{(\w+)\}"`. Množinu znaků, který odpovídá `\w` je přísnější než XML a mnoho zdrojů konfiguraci umožňují. Použití `tokenPattern` při více znaků, než `@"\$\{(\w+)\}"` jsou nezbytné název tokenu.

`tokenPattern`: Řetězec:

* Umožňuje vývojářům měnit regulární výraz, který se použije k porovnání s tokenu.
* Abyste měli jistotu, že je ve správném formátu, nebezpečné regulární výraz se provádí bez ověření.
* Musí obsahovat skupinu zachycení. Celý regulární výraz se musí shodovat celý token. První zachycení musí být název tokenu k vyhledání ve zdroji konfigurace.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Konfigurace počítačů v Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Je nejjednodušší konfiguraci počítačů.
* Čte hodnoty z prostředí.
* Nemá žádné další možnosti konfigurace.
* `name` Hodnota atributu je volitelný.

**Poznámka:** v prostředí kontejneru Windows, jsou proměnné nastavené v době běhu pouze vloženy do prostředí vstupního bodu procesu. Aplikace, na kterých běží jako služba nebo bez vstupního bodu procesu není vyzvednutí tyto proměnné není-li jinak vloží mechanismem v kontejneru. Pro [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)– na základě kontejnerů, aktuální verze [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) zpracovává to *DefaultAppPool* pouze. Ostatní varianty kontejnerů na základě Windows může být potřeba vyvíjet své vlastní mechanismus vkládání pro procesy bez vstupního bodu.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nikdy ukládat hesla, citlivé připojovací řetězce nebo jiných citlivých dat. ve zdrojovém kódu. Tajné klíče v produkčním prostředí by se neměly pro vývoj nebo testování.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Tvůrce tato konfigurace poskytuje podobné funkce [ASP.NET Core tajný klíč správce](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) je možné použít v projektech .NET Framework, ale je třeba zadat soubor tajných kódů. Alternativně můžete definovat `UserSecretsId` vlastnost v projektu soubor a vytvořte soubor raw tajné kódy ve správném umístění pro čtení. Zachovat externí závislosti mimo váš projekt, soubor tajného kódu je ve formátu XML. Formát XML je k implementaci se týká a formát byste se neměli spoléhat. Pokud je potřeba sdílet *secrets.json* soubor s projekty .NET Core, zvažte použití [SimpleJsonConfigBuilder](#simplejsonconfig). `SimpleJsonConfigBuilder` Formátování pro .NET Core byste také zvážit podrobnost implementace může změnit.

Konfigurace atributů pro `UserSecretsConfigBuilder`:

* `userSecretsId` – Toto je upřednostňovanou metodou pro určení souboru XML tajných kódů. Pracuje podobně až po .NET Core, který používá `UserSecretsId` pro uložení tohoto identifikátoru vlastnosti projektu. Řetězec musí být jedinečný, nemusí být identifikátor GUID. Pomocí tohoto atributu `UserSecretsConfigBuilder` dobře známé místní umístění (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) pro soubor tajných kódů, které patří do tohoto identifikátoru.
* `userSecretsFile` -Volitelný atribut určení souboru, který obsahuje tajné klíče. `~` Znak lze použít na začátku tak, aby odkazovaly kořenový adresář aplikace. Buď tento atribut nebo `userSecretsId` atribut je vyžadován. Pokud jsou zadány oba `userSecretsFile` přednost.
* `optional`: logická hodnota, výchozí hodnota `true` – brání výjimku, pokud nelze najít soubor tajných kódů. 
* `name` Hodnota atributu je volitelný.

Soubor tajných kódů má následující formát:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) čte hodnoty uložené v [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` je vyžadováno (název trezoru), nebo identifikátor URI pro trezor. Povolit řízení, o které trezoru pro připojení k další atributy, ale jsou nutné jenom v případě aplikace není spuštěna v prostředí, která funguje s `Microsoft.Azure.Services.AppAuthentication`. Knihovny ověřování služby Azure se používá k automaticky získávají informace o připojení z prostředí pro spouštění Pokud je to možné. Můžete přepsat automaticky vyzvednutí informace o připojení tím, že poskytuje připojovací řetězec.

* `vaultName` – Požadováno pokud `uri` v není k dispozici. Určuje název úložiště ve vašem předplatném Azure, ze kterého se mají číst páry klíč/hodnota.
* `connectionString` – Připojovací řetězec použitelný [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Připojení s ostatními poskytovateli služby Key Vault se zadaným `uri` hodnotu. Pokud není zadán, Azure (`vaultName`) je poskytovatel trezoru.
* `version` -Azure Key Vault poskytuje funkce správy verzí pro tajné kódy. Pokud `version` není zadána, Tvůrce pouze načte tajných kódů odpovídající této verze.
* `preloadSecretNames` – Ve výchozím nastavení tato querys Tvůrce **všechny** klíče názvy v trezoru klíčů po jeho inicializaci. Pokud chcete zabránit, všechny hodnoty klíče pro čtení, tento atribut nastavte na `false`. Nastavení na `false` přečte tajných kódů jeden po druhém. Tajné kódy, které postupně po jednom můžete užitečné, pokud trezor umožňuje přístup "Get" ale ne "seznam" přístup ke čtení. **Poznámka:** při použití `Greedy` režimu `preloadSecretNames` musí být `true` (výchozí nastavení.)

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) je základní konfigurace tvůrce, který používá souborů v adresáři jako zdroj hodnoty. Název souboru je klíč a obsah se hodnota. Tvůrce této konfigurace může být užitečné při spuštění v prostředí iniciovat organizovaně, což kontejneru. Systémy, jako je Docker Swarm a Kubernetes poskytují `secrets` na své kontejnery iniciovat organizovaně, což windows tímto způsobem na soubor klíče.

Podrobnosti atributu:

* `directoryPath` -Vyžaduje. Určuje cestu k vyhledání hodnoty. Docker pro Windows se tajné kódy jsou uložené v *C:\ProgramData\Docker\secrets* adresáře ve výchozím nastavení.
* `ignorePrefix` – Soubory, které začínat touto předponou budou vyloučeny. Výchozí hodnota je "ignorovat.".
* `keyDelimiter` – Výchozí hodnota je `null`. -Li zadána, Tvůrce konfigurace prochází přes několik úrovní adresáře, vytváření názvy klíčů s tímto oddělovačem. Pokud je tato hodnota `null`, Tvůrce konfigurace prohledá pouze na nejvyšší úrovni adresáře.
* `optional` – Výchozí hodnota je `false`. Určuje, zda by měl Tvůrce konfigurace způsobit chyby, pokud zdrojový adresář neexistuje.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nikdy ukládat hesla, citlivé připojovací řetězce nebo jiných citlivých dat. ve zdrojovém kódu. Tajné klíče v produkčním prostředí by se neměly pro vývoj nebo testování.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Projekty .NET core často používají pro konfigurační soubory JSON. [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Tvůrce umožňuje soubory .NET Core JSON pro použití v rozhraní .NET Framework. Tvůrce této konfigurace je poskytuje základní mapování ze zdroje bez stromové struktury klíč/hodnota do oblasti konkrétní klíč/hodnota konfigurace rozhraní .NET Framework. Tato konfigurace Tvůrce nemá **není** poskytují pro hierarchické konfigurace. Základní soubor JSON je podobný slovník, není komplexní hierarchický objekt. Víceúrovňových hierarchických soubor lze použít. Tento zprostředkovatel `flatten`s hloubkou připojením názvu vlastnosti na každé úrovni pomocí `:` jako oddělovač.

Podrobnosti atributu:

* `jsonFile` -Vyžaduje. Určuje soubor JSON, který se má načíst z. `~` Znak je možné při spuštění aplikace kořen odkazu.
* `optional` -Logická hodnota, výchozí hodnota je `true`. Brání generování výjimek, pokud nelze najít soubor JSON.
* `jsonMode` - `[Flat|Sectional]`. `Flat` je výchozí nastavení. Když `jsonMode` je `Flat`, soubor JSON je zdrojem plochý klíč/hodnota. `EnvironmentConfigBuilder` a `AzureKeyVaultConfigBuilder` jsou také zdrojů bez stromové struktury klíč/hodnota. Když `SimpleJsonConfigBuilder` je nakonfigurovaný v `Sectional` režimu:

  * Soubor JSON je jenom na nejvyšší úrovni koncepčně rozdělen do více slovníky.
  * Každý z slovníků platí jenom pro konfigurační oddíl, který odpovídá názvu vlastností nejvyšší úrovně připojená k nim. Příklad:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementace Tvůrce vlastní klíč/hodnota konfigurace

Pokud konfigurace tvůrci nevyhovují vašim potřebám, můžete napsat vlastní. `KeyValueConfigBuilder` Základní třídy se stará o nahrazení režimy a většina obavy předponu. Projekt implementující potřebovat pouze:

* Dědit ze základní třídy a implementovat základní příčiny páry klíč/hodnota pomocí `GetValue` a `GetAllValues`:
* Přidat [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) do projektu.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

`KeyValueConfigBuilder` Základní třída poskytuje většinu práce a konzistentní chování napříč klíč/hodnota konfigurace počítačů.

## <a name="additional-resources"></a>Další zdroje

* [Úložiště GitHub tvůrci konfigurace](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Ověřování služba služba do služby Azure Key Vault pomocí rozhraní .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
