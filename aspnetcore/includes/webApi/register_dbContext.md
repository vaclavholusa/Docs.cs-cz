## <a name="register-the-database-context"></a>Zaregistrovat kontext databáze

Chcete-li vložit kontext databáze do kontroleru, potřebujeme a zaregistrujte ho pomocí [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru. Zaregistrovat kontext databáze kontejneru služby pomocí integrovanou podporu pro [vkládání závislostí](xref:fundamentals/dependency-injection). Nahraďte obsah *Startup.cs* soubor s následující:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Předchozí kód:

* Odebere kód, který jsme nepoužívají.
* Určuje, že databázi v paměti je vloženy do kontejneru služby.
