---
title: "Poskytovatel konfigurace služby Azure Key Vault"
author: guardrex
description: "Další informace o použití konfigurace zprostředkovatele služby Azure klíč trezoru pro konfiguraci aplikace pomocí dvojice název hodnota načíst za běhu."
manager: wpickett
ms.author: riande
ms.date: 08/09/2017
ms.prod: asp.net-core
ms.topic: article
uid: security/key-vault-configuration
ms.openlocfilehash: 1318ae855154dd8fc91ff0c19b0ab111d86c71e6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="azure-key-vault-configuration-provider"></a>Poskytovatel konfigurace služby Azure Key Vault

Podle [Luke Latham](https://github.com/guardrex) a [Andrew Stanton-Nurse](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)

Zobrazit nebo stáhnout ukázkový kód pro 2.x:

* [Základní ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([stažení](xref:tutorials/index#how-to-download-a-sample))-čte tajný hodnoty do aplikace.
* [Název klíče předponu ukázkové](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([stažení](xref:tutorials/index#how-to-download-a-sample)) – čtení tajný hodnoty pomocí předpony název klíče, který představuje verzi aplikace, což vám umožní načíst jinou sadu tajný hodnoty pro každou verzi aplikace.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Zobrazit nebo stáhnout ukázkový kód pro 1.x:

* [Základní ukázka](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([stažení](xref:tutorials/index#how-to-download-a-sample))-čte tajný hodnoty do aplikace.
* [Název klíče předponu ukázkové](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([stažení](xref:tutorials/index#how-to-download-a-sample)) – čtení tajný hodnoty pomocí předpony název klíče, který představuje verzi aplikace, což vám umožní načíst jinou sadu tajný hodnoty pro každou verzi aplikace. 

---

Tento dokument vysvětluje, jak používat [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) poskytovatele konfigurace pro načtení hodnoty konfigurace aplikace z Azure Key Vault tajných klíčů. Azure Key Vault je Cloudová služba, která pomáhá chránit kryptografické klíče a tajné klíče používané aplikace a služby. Řízení přístupu k citlivým konfigurační data mezi obvyklé scénáře patří a splňuje požadavek na FIPS 140-2 Level 2 ověřit moduly hardwarového zabezpečení (HSM) při ukládání konfigurační data. Tato funkce je k dispozici pro aplikace, které cílí ASP.NET Core 1.1 nebo vyšší.

## <a name="package"></a>Balíček
Chcete-li použít poskytovatele, přidejte odkaz na [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) balíčku.

## <a name="application-configuration"></a>Konfigurace aplikace
Můžete si prostudovat zprostředkovatele s [ukázkové aplikace](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Po vytvoření trezoru klíčů a vytvoření tajných klíčů v trezoru, ukázkových aplikací bezpečně načíst tajný hodnoty do jejich konfigurace a jejich zobrazení na webových stránkách.

Zprostředkovatel je přidán do `ConfigurationBuilder` s `AddAzureKeyVault` rozšíření. V ukázkových aplikací rozšíření používá tři načíst z hodnoty konfigurace *appSettings.JSON určený* souboru.

| Nastavení aplikace    | Popis                    | Příklad                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Název Azure Key Vault           | contosovault                                 |
| `ClientId`     | Id aplikace Azure Active Directory  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Klíč aplikace Azure Active Directory | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Vytvoření trezoru klíčů tajných klíčů a načítání hodnoty konfigurace (basic – ukázka)
1. Vytvoření trezoru klíčů a nastavení Azure Active Directory (Azure AD) pro aplikaci následující pokyny v [Začínáme s Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Přidat tajné klíče do trezoru klíčů pomocí [modulu PowerShell trezoru klíč AzureRM](/powershell/module/azurerm.keyvault) dostupné z [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [REST API služby Azure Key Vault](/rest/api/keyvault/), nebo [Portál azure](https://portal.azure.com/). Tajné klíče byly vytvořeny jako buď *ruční* nebo *certifikát* tajných klíčů. *Certifikát* tajné klíče jsou certifikáty používané aplikace a služby, ale nejsou podporovány ve zprostředkovateli konfigurace. Měli byste použít *ruční* možnost vytvořit tajné klíče dvojice název hodnota pro použití se zprostředkovatelem konfigurace.
    * Jednoduché tajné klíče jsou vytvořené jako dvojice název hodnota. Azure Key Vault názvů tajných klíčů jsou omezeny na alfanumerické znaky a spojovníky.
    * Hierarchická hodnoty (konfigurační oddíly) použijte `--` (dvě pomlčky) jako oddělovač v ukázce. Použití dvojteček, které se obvykle používají pro vymezení části z podklíč v [konfigurace ASP.NET Core](xref:fundamentals/configuration/index), nejsou povoleny v názvů tajných klíčů. Proto jsou dvě pomlčky používá a vzájemně zaměněny pro dvojtečkou při těchto tajných klíčů jsou načteny do konfigurace aplikace.
    * Vytvořte dvě *ruční* tajných klíčů s následující páry název hodnota. První tajný klíč je jednoduchý název a hodnotu a druhý tajný klíč vytvoří tajná hodnota s části a podklíč v tajný název:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Ukázková aplikace zaregistrujte v Azure Active Directory.
  * Autorizovat aplikaci pro přístup k trezoru klíčů. Při použití `Set-AzureRmKeyVaultAccessPolicy` rutiny prostředí PowerShell autorizovat aplikaci pro přístup k trezoru klíčů, zadejte `List` a `Get` přístup k tajných klíčů s `-PermissionsToSecrets list,get`.
2. Aktualizace aplikace služby *appSettings.JSON určený* souboru s hodnotami `Vault`, `ClientId`, a `ClientSecret`.
3. Spuštění ukázkové aplikace, která získává jeho hodnoty konfigurace z `IConfigurationRoot` se stejným názvem jako tajný název.
  * Hierarchická bez hodnoty: hodnota `SecretName` se získávají pomocí `config["SecretName"]`.
  * Hierarchická hodnoty (oddíly): použití `:` zápis (dvojtečka) nebo `GetSection` metoda rozšíření. Použijte jednu z těchto přístupů k získání hodnoty konfigurace:
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

Při spuštění aplikace zobrazuje webová stránka načtená tajný hodnoty:

![Okno prohlížeče zobrazující tajný hodnoty načíst prostřednictvím poskytovatele konfigurace Azure klíč trezoru](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Vytvoření tajných klíčů předponou trezoru klíčů a načítání hodnoty konfigurace (klíč název předpona – ukázka)
`AddAzureKeyVault`také poskytuje přetížení, které přijímá implementace `IKeyVaultSecretManager`, která umožňuje řídit způsob klíče tajné klíče trezoru se převedou na konfigurační klíče. Například můžete implementovat rozhraní načíst tajný hodnot na základě předpony hodnoty, které poskytnete při spuštění aplikace. To vám například umožňuje načíst tajné klíče založené na verzi aplikace.

> [!WARNING]
> Nepoužívejte předpony na tajné klíče trezoru klíčů, umístit do stejné trezoru klíčů tajné klíče pro víc aplikací nebo umístit prostředí tajné klíče (například *vývoj* verus *produkční* tajné klíče) do stejné trezor. Doporučujeme různé aplikace a vývoj nebo produkční prostředí použít samostatné trezorů klíčů izolovat aplikace prostředí pro nejvyšší úroveň zabezpečení.

Pomocí druhého ukázkovou aplikaci, vytvořte tajný klíč v trezoru klíčů pro `5000-AppSecret` (tečky nejsou povoleny v trezoru klíčů názvů tajných klíčů) představující tajný klíč aplikace pro 5.0.0.0 verzi vaší aplikace. Pro jinou verzi, 5.1.0.0, vytvoříte tajný klíč pro `5100-AppSecret`. Jednotlivé verze aplikace načítá vlastní tajná hodnota do jeho konfiguraci jako `AppSecret`, vypuzovacího vypnout verze načtenou tajný klíč. Implementace tohoto příkladu je zobrazena níže:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Metoda je volána algoritmem zprostředkovatele, který iteruje tajné klíče trezoru najít ty, které mají předponu verze. Když najde předponu verze s `Load`, používá algoritmus `GetKey` metoda vrátí název konfigurace tajný název. Ji odstraní vypnout předponu verze z názvu tajného klíče a vrátí zbytek tajný název pro načítání do konfigurace aplikace dvojice název hodnota.

Při implementaci tohoto přístupu:

1. Tajné klíče trezoru klíčů jsou načteny.
2. Řetězec tajný klíč pro `5000-AppSecret` je nalezena shoda.
3. Verze, `5000` (s pomlčkou), se odstraní z název klíče a `AppSecret` načtení s tajná hodnota do konfigurace aplikace.

> [!NOTE]
> Můžete taky zadat vlastní `KeyVaultClient` implementace k `AddAzureKeyVault`. Zadávání vlastního klienta můžete sdílet jednu instanci klienta mezi zprostředkovatele konfigurace a dalších částí vaší aplikace.

1. Vytvoření trezoru klíčů a nastavení Azure Active Directory (Azure AD) pro aplikaci následující pokyny v [Začínáme s Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Přidat tajné klíče do trezoru klíčů pomocí [modulu PowerShell trezoru klíč AzureRM](/powershell/module/azurerm.keyvault) dostupné z [Galerie prostředí PowerShell](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [REST API služby Azure Key Vault](/rest/api/keyvault/), nebo [Portál azure](https://portal.azure.com/). Tajné klíče byly vytvořeny jako buď *ruční* nebo *certifikát* tajných klíčů. *Certifikát* tajné klíče jsou certifikáty používané aplikace a služby, ale nejsou podporovány ve zprostředkovateli konfigurace. Měli byste použít *ruční* možnost vytvořit tajné klíče dvojice název hodnota pro použití se zprostředkovatelem konfigurace.
    * Hierarchická hodnoty (konfigurační oddíly) použijte `--` (dvě pomlčky) jako oddělovač.
    * Vytvořte dvě *ruční* tajných klíčů s následující páry název hodnota:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Ukázková aplikace zaregistrujte v Azure Active Directory.
  * Autorizovat aplikaci pro přístup k trezoru klíčů. Při použití `Set-AzureRmKeyVaultAccessPolicy` rutiny prostředí PowerShell autorizovat aplikaci pro přístup k trezoru klíčů, zadejte `List` a `Get` přístup k tajných klíčů s `-PermissionsToSecrets list,get`.
2. Aktualizace aplikace služby *appSettings.JSON určený* souboru s hodnotami `Vault`, `ClientId`, a `ClientSecret`.
3. Spuštění ukázkové aplikace, která získává jeho hodnoty konfigurace z `IConfigurationRoot` se stejným názvem jako předponou tajný název. V této ukázce předponu je verze aplikace, která jste zadali na `PrefixKeyVaultSecretManager` když jste přidali poskytovatel konfigurace Azure Key Vault. Hodnota `AppSecret` se získávají pomocí `config["AppSecret"]`. Webová stránka generované aplikace zobrazuje načíst hodnotu:

   ![Okno prohlížeče zobrazující tajná hodnota načíst prostřednictvím poskytovatele konfigurace Azure klíč trezoru, pokud je 5.0.0.0 verze aplikace](key-vault-configuration/_static/sample2-1.png)

4. Změna verze sestavení aplikace v souboru projektu z `5.0.0.0` k `5.1.0.0` a znovu spusťte aplikaci. Tato doba tajná hodnota vrácená je `5.1.0.0_secret_value`. Webová stránka generované aplikace zobrazuje načíst hodnotu:

   ![Okno prohlížeče zobrazující tajná hodnota načíst prostřednictvím poskytovatele konfigurace Azure klíč trezoru, pokud je 5.1.0.0 verze aplikace](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Řízení přístupu ke ClientSecret
Použití [nástroj tajný klíč správce](xref:security/app-secrets) udržovat `ClientSecret` mimo vašeho projektu zdroj stromu. Pomocí nástroje Správce tajný klíč, můžete přidružit konkrétní projekt tajné klíče aplikace a sdílet je ve více projektech.

Při vývoji aplikace rozhraní .NET Framework v prostředí, které podporuje certifikáty, můžete ověřovat do Azure Key Vault společně s certifikátem X.509. Privátní klíč certifikátu X.509 spravuje operačního systému. Další informace najdete v tématu [ověřit pomocí certifikátu místo tajný klíč klienta](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Použití `AddAzureKeyVault` přetížení, které přijímá `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Opětovné načtení tajné klíče
Tajné klíče ukládají do mezipaměti, dokud `IConfigurationRoot.Reload()` je volána. Platnost vypršela, zakázané, a aplikace, dokud nejsou dodržovány aktualizované tajných klíčů v trezoru klíčů `Reload` se spustí.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Zakázaná a vypršela platnost tajné klíče
Throw tajných klíčů zakázaná a vypršela platnost `KeyVaultClientException`. Abyste zabránili vyvolání aplikace, nahraďte aplikace nebo aktualizovat zakázána nebo platnost tajný klíč.

## <a name="troubleshooting"></a>Poradce při potížích
Jakmile se aplikace se nepodaří načíst konfiguraci pomocí poskytovatele, chybová zpráva zapsány do [ASP.NET protokolování infrastruktury](xref:fundamentals/logging/index). Tyto podmínky zabrání konfiguraci z načítání:
* Aplikace v Azure Active Directory není správně nakonfigurován.
* Trezor klíčů neexistuje v Azure Key Vault.
* Aplikace nemá oprávnění pro přístup k trezoru klíčů.
* Zásady přístupu neobsahuje `Get` a `List` oprávnění.
* V trezoru klíčů konfigurační data (dvojice název hodnota) je nesprávně s názvem, chybí, zakázána, nebo vypršela platnost.
* Aplikace má nesprávný trezoru klíčů název (`Vault`), Id aplikace Azure AD (`ClientId`), nebo klíč služby Azure AD (`ClientSecret`).
* Klíč služby Azure AD (`ClientSecret`) vypršela.
* Konfigurační klíč (název) je nesprávný v aplikaci pro hodnotu, kterou se pokoušíte načíst.

## <a name="additional-resources"></a>Další zdroje
* [Konfigurace](xref:fundamentals/configuration/index)
* [Microsoft Azure: Trezor klíčů](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Dokumentaci k trezoru klíčů](https://docs.microsoft.com/azure/key-vault/)
* [Klíče postup generování a přenos chráněných pomocí HSM pro Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient – třída](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
