# <a name="key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Zprostředkovatel konfigurace trezoru klíčů ukázkovou aplikaci (ASP.NET Core 1.x)

Tento příklad ukazuje použití zprostředkovatele služby Azure Key Vault konfigurace pro ASP.NET Core 1.x. Ukázka 2.x ASP.NET Core, najdete v části [zprostředkovatele konfigurace trezoru klíčů ukázkovou aplikaci (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x).

> [!NOTE]
> Poskytovatel konfigurace není k dispozici pro ASP.NET Core 1.0. Pokud chcete implementovat zprostředkovatele konfigurace a aplikace jsou aplikace ASP.NET Core 1.0, upgradujte aplikaci na první 1.1 nebo novější.

Další informace o tom, jak ukázku funguje, najdete v článku [poskytovatele konfigurace Azure Key Vault](xref:security/key-vault-configuration) tématu.

## <a name="using-the-sample"></a>Pomocí vzorku
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
