# <a name="how-to-buildrun-secure-user-data-sample"></a>Postup sestavení a spuštění ukázkových dat zabezpečení uživatele

* Nastavení hesla pomocí nástroje tajný klíč správce:

  `dotnet user-secrets set SeedUserPW <pw>`

* Aktualizace databáze:

    `dotnet ef database update`
