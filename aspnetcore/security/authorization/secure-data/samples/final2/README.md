# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="eae4a-101">Postup sestavení a spuštění ukázkových dat zabezpečení uživatele</span><span class="sxs-lookup"><span data-stu-id="eae4a-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="eae4a-102">Nastavení hesla pomocí nástroje tajný klíč správce:</span><span class="sxs-lookup"><span data-stu-id="eae4a-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="eae4a-103">Aktualizace databáze:</span><span class="sxs-lookup"><span data-stu-id="eae4a-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="eae4a-104">Povolit protokol SSL v projektu</span><span class="sxs-lookup"><span data-stu-id="eae4a-104">Enable SSL in the project</span></span>