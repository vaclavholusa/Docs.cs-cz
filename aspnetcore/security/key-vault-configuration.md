---
title: Zprostředkovatel konfigurace Azure Key Vault v ASP.NET Core
author: guardrex
description: Zjistěte, jak nakonfigurovat aplikaci pomocí dvojice název hodnota v době běhu načteny pomocí zprostředkovatele konfigurace trezoru klíčů Azure.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/17/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 474824cccdc63bb3dc3978ed68cf4c89cec12ad5
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391139"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Zprostředkovatel konfigurace Azure Key Vault v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex) a [Andrew Stanton sestry](https://github.com/anurse)

Tento dokument popisuje, jak používat [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) poskytovatel konfigurace pro načtení hodnoty konfigurace aplikace z Azure Key Vault tajných kódů. Azure Key Vault je Cloudová služba, která pomáhá chránit kryptografické klíče a tajné klíče používané aplikacemi a službami. Řízení přístupu k citlivým konfigurační data patří běžné scénáře a splňuje požadavek na FIPS 140-2 úrovně 2 ověřit modulů hardwarového zabezpečení (HSM) při ukládání konfigurační data. Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo vyšší.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="package"></a>Balíček

Pokud chcete použít poskytovatele, přidejte odkaz na [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) balíčku.

## <a name="app-configuration"></a>Konfigurace aplikace

Můžete prozkoumat zprostředkovatele s [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Po vytvoření služby key vault a vytvořte tajných kódů v trezoru, ukázkové aplikace bezpečně načtení hodnoty tajných kódů do jejich konfigurace a jejich zobrazení na webových stránkách.

Zprostředkovatel se přidá do konfigurace aplikace s `AddAzureKeyVault` rozšíření. V ukázkových aplikací, rozšíření používá tři načteny z hodnot konfigurace *appsettings.json* souboru.

| Nastavení aplikace    | Popis                    | Příklad                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Název služby Azure Key Vault           | contosovault                                 |
| `ClientId`     | Id aplikace Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Klíč aplikace Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Vytvoření tajných kódů služby key vault a načíst hodnoty konfigurace (ukázka basic)

1. Vytvoření služby key vault a nastavení Azure Active Directory (Azure AD) pro aplikaci následující pokyny v [Začínáme s Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Přidávání tajných kódů pomocí služby key vault [modulu AzureRM Key Vault prostředí PowerShell](/powershell/module/azurerm.keyvault) k dispozici [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [REST API služby Azure Key Vault](/rest/api/keyvault/), nebo [Webu azure Portal](https://portal.azure.com/). Tajné kódy jsou vytvořeny jako buď *ruční* nebo *certifikát* tajných kódů. *Certifikát* tajné kódy jsou certifikáty pro použití aplikacemi a službami, ale nepodporuje zprostředkovatele konfigurace. Měli byste použít *ruční* možnost vytvořit tajné klíče dvojice název hodnota pro použití se zprostředkovatelem konfigurace.
     * Jednoduché tajné kódy jsou vytvořeny jako dvojice název hodnota. Azure Key Vault tajný názvy jsou omezené na alfanumerické znaky a pomlčky.
     * Hierarchické hodnoty (konfigurační oddíly) použijte `--` (dvě pomlčky) jako oddělovač v ukázce. Použití dvojteček, které se obvykle používají pro vymezení část z podklíč v [konfigurace ASP.NET Core](xref:fundamentals/configuration/index), nejsou povoleny v názvů tajných klíčů. Proto jsou použít dvě pomlčky a má Prohodit pro dvojtečkou tajné klíče jsou načtena do konfigurace aplikace.
     * Pak vytvoříte další dva *ruční* tajných kódů pomocí následující dvojice název hodnota. První tajný kód je jednoduchý název a hodnotu, a druhý tajný kód vytvoří tajná hodnota s části a v názvu tajný podklíč:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Zaregistrujte ukázkovou aplikaci v Azure Active Directory.
   * Povolit aplikaci přístup k trezoru klíčů. Při použití `Set-AzureRmKeyVaultAccessPolicy` rutinu Powershellu autorizovat aplikaci přistupovat k trezoru klíčů, poskytují `List` a `Get` přístup k tajným kódům s `-PermissionsToSecrets list,get`.

2. Aktualizace aplikace *appsettings.json* souboru s hodnotami `Vault`, `ClientId`, a `ClientSecret`.
3. Spuštění ukázkové aplikace, která získává svůj konfigurační hodnoty z `IConfigurationRoot` se stejným názvem jako název tajného kódu.
   * Hierarchické bez hodnoty: hodnota `SecretName` se získá pomocí `config["SecretName"]`.
   * Hierarchické hodnoty (oddílů): použijte `:` zápis (dvojtečka) nebo `GetSection` – metoda rozšíření. Použijte některou z těchto přístupů k získání hodnoty konfigurace:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Při spuštění aplikace, webová stránka zobrazuje načíst hodnoty tajných kódů:

![Okno prohlížeče zobrazující hodnoty tajných kódů, které jsou načteny prostřednictvím poskytovatele konfigurace Azure Key Vault](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a>Svázat pole třídy

Zprostředkovatel je schopný načíst konfigurační hodnoty do pole pro vazbu k poli POCO.

Při čtení ze zdroje konfigurace, která umožňuje klíče obsahovat dvojtečku (`:`) oddělovače, číselné klíčové segment, který se používá k rozlišení klíčů, které tvoří pole (`:0:`, `:1:`;... `:{n}:`). Další informace najdete v tématu [konfigurace: svázat pole třídy](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Azure Key Vault klíče nelze použít dvojtečku jako oddělovač. Postupu popsaného v tomto tématu používá dvojité pomlčky (`--`) jako oddělovač pro hierarchické hodnoty (oddíly). Pole klíče jsou uložené ve službě Azure Key Vault s double pomlčky a číselné klíčových segmentů (`--0--`, `--1--`;... `--{n}--`).

Zkontrolujte následující [Serilog](https://serilog.net/) protokolování konfigurace poskytovatele poskytovaný souborem JSON. Existují dva literály definované v objektu `WriteTo` pole, které zahrnují dva Serilog *jímky*, které popisují cíle pro výstup protokolování:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

Konfigurace je znázorněno v předchozím soubor JSON je uložená ve službě Azure Key Vault pomocí dvojitá čárka (`--`) zápisem a číselné segmenty:

| Key | Hodnota |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Vytvoření tajných kódů předponou služby key vault a načíst hodnoty konfigurace (klíč název předpona sample)

`AddAzureKeyVault` také poskytuje přetížení přijímající implementace `IKeyVaultSecretManager`, což vám umožňuje řídit jak klíče trezoru tajných kódů se převedou na konfigurační klíče. Například můžete implementovat rozhraní za účelem načtení hodnoty tajných kódů na základě hodnoty předpony, které poskytnete při spuštění aplikace. Díky tomu můžete například načíst tajné kódy na základě verze aplikace.

> [!WARNING]
> Nepoužívejte předpony v tajných kódů služby key vault, umístí tajných kódů pro více aplikací do stejného trezoru klíčů nebo umístit prostředí tajné kódy (například *vývoj* oproti *produkční* tajné kódy) do jedné trezor. Doporučujeme různých aplikací a vývojové nebo produkční prostředí použít samostatné trezorům klíčů k izolování aplikace prostředí pro nejvyšší úroveň zabezpečení.

Pomocí druhého ukázkovou aplikaci, vytvoření tajného klíče v trezoru klíčů pro `5000-AppSecret` (tečky nejsou povoleny v názvech tajných kódů služby key vault) představující tajný kód aplikace pro 5.0.0.0 verzi vaší aplikace. Pro jinou verzi, 5.1.0.0, vytvoření tajného klíče pro `5100-AppSecret`. Každá verze aplikace načte do jeho konfiguraci jako vlastní tajná hodnota `AppSecret`, vypuzovacího vypnout verze během načítání tajného klíče. Ukázková implementace je zobrazena níže:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Metoda je volána metodou poskytovatele algoritmus, který Iteruje přes našli ty, které mají předponu verze tajné klíče trezoru. Když se nachází verze předpony s `Load`, používá algoritmus `GetKey` metoda vrátí název konfigurace název tajného kódu. Odstraní vypnout verze předpony z názvu tajného klíče a vrátí zbytek název tajného kódu pro načtení do konfigurace aplikace dvojice název hodnota.

Při implementaci tohoto přístupu:

1. Tajné klíče služby key vault se načítají.
2. Řetězec tajný klíč pro `5000-AppSecret` je nalezena shoda.
3. Verze, `5000` (s pomlčkou), je odebrána z názvu klíče byste museli opustit `AppSecret` načíst s hodnota tajného klíče do konfigurace aplikace.

> [!NOTE]
> Můžete taky zadat vlastní `KeyVaultClient` implementaci `AddAzureKeyVault`. Zadání vlastního klienta umožňuje sdílet jednu instanci klienta mezi zprostředkovatele konfigurace a ostatní části vaší aplikace.

1. Vytvoření služby key vault a nastavení Azure Active Directory (Azure AD) pro aplikaci následující pokyny v [Začínáme s Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Přidávání tajných kódů pomocí služby key vault [modulu AzureRM Key Vault prostředí PowerShell](/powershell/module/azurerm.keyvault) k dispozici [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [REST API služby Azure Key Vault](/rest/api/keyvault/), nebo [Webu azure Portal](https://portal.azure.com/). Tajné kódy jsou vytvořeny jako buď *ruční* nebo *certifikát* tajných kódů. *Certifikát* tajné kódy jsou certifikáty pro použití aplikacemi a službami, ale nepodporuje zprostředkovatele konfigurace. Měli byste použít *ruční* možnost vytvořit tajné klíče dvojice název hodnota pro použití se zprostředkovatelem konfigurace.
     * Hierarchické hodnoty (konfigurační oddíly) použijte `--` (dvě pomlčky) jako oddělovač.
     * Pak vytvoříte další dva *ruční* tajných kódů pomocí následující dvojice název hodnota:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Zaregistrujte ukázkovou aplikaci v Azure Active Directory.
   * Povolit aplikaci přístup k trezoru klíčů. Při použití `Set-AzureRmKeyVaultAccessPolicy` rutinu Powershellu autorizovat aplikaci přistupovat k trezoru klíčů, poskytují `List` a `Get` přístup k tajným kódům s `-PermissionsToSecrets list,get`.

2. Aktualizace aplikace *appsettings.json* souboru s hodnotami `Vault`, `ClientId`, a `ClientSecret`.
3. Spuštění ukázkové aplikace, která získává svůj konfigurační hodnoty z `IConfigurationRoot` se stejným názvem jako předponou název tajného kódu. V této ukázce je předpona, která verze aplikace, které jste zadali do `PrefixKeyVaultSecretManager` při přidání poskytovatele konfigurace služby Azure Key Vault. Hodnota pro `AppSecret` se získá pomocí `config["AppSecret"]`. Webová stránka vygenerovaný touto aplikací zobrazí načíst hodnotu:

   ![Okno prohlížeče zobrazující tajná hodnota načteného prostřednictvím zprostředkovatele konfigurace trezoru klíčů Azure, pokud je verze aplikace 5.0.0.0](key-vault-configuration/_static/sample2-1.png)

4. Změnit verzi sestavení aplikace v souboru projektu z `5.0.0.0` k `5.1.0.0` a znovu spusťte aplikaci. Tentokrát, vrácená hodnota tajného kódu je `5.1.0.0_secret_value`. Webová stránka vygenerovaný touto aplikací zobrazí načíst hodnotu:

   ![Okno prohlížeče zobrazující tajná hodnota načteného prostřednictvím zprostředkovatele konfigurace trezoru klíčů Azure, pokud je verze aplikace 5.1.0.0](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>Řízení přístupu k ClientSecret

Použití [nástroj tajný klíč správce](xref:security/app-secrets) udržovat `ClientSecret` mimo projekt stromu zdrojového kódu. Pomocí manažera tajných přidružit k určitému projektu tajných kódů aplikace a sdílet mezi více projekty.

Při vývoji aplikace rozhraní .NET Framework v prostředí, které podporuje certifikáty, můžete ověřovat do služby Azure Key Vault pomocí certifikátu X.509. Privátní klíč certifikátu X.509 se spravuje přes operační systém. Další informace najdete v tématu [ověřování pomocí certifikátu místo tajného klíče klienta](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Použití `AddAzureKeyVault` přetížení přijímající `X509Certificate2` (`_env` v následujícím příkladu:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>Znovu načíst tajné kódy

Tajné klíče jsou uložené v mezipaměti až do `IConfigurationRoot.Reload()` je volána. Vypršela platnost, zakázané, a aktualizované tajné kódy ve službě key vault není respektována aplikace do `Reload` provádí.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Tajné kódy zakázaná a jejichž platnost vypršela

Vyvolat zakázaná a jejichž platnost vypršela tajných kódů `KeyVaultClientException`. Abyste zabránili vyvolání vaší aplikace, nahradit aplikaci nebo aktualizovat tajný kód zakázaný nebo vypršela platnost.

## <a name="troubleshoot"></a>Řešení potíží

Když aplikaci se pak nepodaří načíst konfiguraci pomocí zprostředkovatele, chybová zpráva je zapsána do [ASP.NET Core protokolování infrastruktury](xref:fundamentals/logging/index). Konfigurace načítání nebudou moct tyto podmínky:

* Aplikace není správně nakonfigurovaný v Azure Active Directory.
* Trezor klíčů neexistuje ve službě Azure Key Vault.
* Aplikace nemá oprávnění pro přístup k trezoru klíčů.
* Zásady přístupu neobsahuje `Get` a `List` oprávnění.
* Ve službě key vault konfigurační data (dvojice název hodnota) je nesprávně pojmenované, chybí, zakázán, nebo vypršela platnost.
* Aplikace má název chybný trezoru klíčů (`Vault`), Id aplikace Azure AD (`ClientId`), nebo klíč služby Azure AD (`ClientSecret`).
* Klíč služby Azure AD (`ClientSecret`) vypršela.
* Konfigurační klíč (název) je nesprávný v aplikaci pro hodnotu, kterou se pokoušíte načíst.

## <a name="additional-resources"></a>Další zdroje

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Trezor klíčů](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Dokumentace ke službě Key Vault](/azure/key-vault/)
* [Postup generování a přenos chráněných pomocí HSM klíčů pro Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [Třída KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
