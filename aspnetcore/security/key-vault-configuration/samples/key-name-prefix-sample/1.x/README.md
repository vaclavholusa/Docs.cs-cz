# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Předpona zprostředkovatel konfigurace trezoru klíčů ukázkovou aplikaci (ASP.NET Core 1.x)

Tento příklad ukazuje použití zprostředkovatele služby Azure Key Vault konfigurace pro ASP.NET Core 1.x pomocí předpony název klíče. Ukázka 2.x ASP.NET Core, najdete v části [předpony zprostředkovatele konfigurace trezoru klíčů ukázkovou aplikaci (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x).

> [!NOTE]
> Poskytovatel konfigurace není k dispozici pro ASP.NET Core 1.0. Pokud chcete implementovat zprostředkovatele konfigurace a aplikace jsou aplikace ASP.NET Core 1.0, upgradujte aplikaci na první 1.1 nebo novější.

Další informace o tom, jak ukázku funguje, najdete v článku [poskytovatele konfigurace Azure Key Vault](xref:security/key-vault-configuration) tématu.

## <a name="using-the-sample"></a>Pomocí vzorku
1. Vytvoření trezoru klíčů a nastavení Azure Active Directory (Azure AD) pro aplikaci následující pokyny v [Začínáme s Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Přidejte tajné klíče do trezoru klíčů pomocí modulu Azure PowerShell, rozhraní API pro správu služby Azure nebo portálu Azure. Tajné klíče byly vytvořeny jako buď *ruční* nebo *certifikát* tajných klíčů. *Certifikát* tajné klíče jsou certifikáty používané aplikace a služby, ale nejsou podporovány ve zprostředkovateli konfigurace. Měli byste použít *ruční* možnost vytvořit tajné klíče dvojice název hodnota pro použití se zprostředkovatelem konfigurace.
    * Hierarchická hodnoty (konfigurační oddíly) použijte `--` (dvě pomlčky) jako oddělovač.
    * Pro ukázkovou aplikaci, vytvořte dvě *ruční* tajných klíčů s následující páry název hodnota:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Ukázková aplikace zaregistrujte v Azure Active Directory.
  * Autorizovat aplikaci pro přístup k trezoru klíčů. Při použití `Set-AzureRmKeyVaultAccessPolicy` rutiny prostředí PowerShell autorizovat aplikaci pro přístup k trezoru klíčů, zadejte `List` a `Get` přístup k tajných klíčů s `-PermissionsToKeys list,get`.
2. Aktualizace aplikace služby *appSettings.JSON určený* souboru s hodnotami `Vault`, `ClientId`, a `ClientSecret`.
3. Spuštění ukázkové aplikace, která získává jeho hodnoty konfigurace z `IConfigurationRoot` se stejným názvem jako předponou tajný název. V této ukázce předponu je verze aplikace, která jste zadali na `PrefixKeyVaultSecretManager` když jste přidali poskytovatel konfigurace Azure Key Vault. Hodnota `AppSecret` se získávají pomocí `config["AppSecret"]`.
4. Změna verze sestavení aplikace v souboru projektu z `5.0.0.0` k `5.1.0.0` a znovu spusťte aplikaci. Tato doba tajná hodnota vrácená je `5.1.0.0_secret_value`.
