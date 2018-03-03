## <a name="register-the-database-context"></a>Zaregistrovat kontext databáze

V tomto kroku není zaregistrována kontext databáze [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru. Služby (například kontext databáze), které jsou registrovány kontejneru pro vkládání (DI) závislosti jsou dostupné v řadičích.

Zaregistrovat kontext databáze kontejneru služby pomocí integrovanou podporu pro [vkládání závislostí](xref:fundamentals/dependency-injection). Nahraďte obsah *Startup.cs* soubor s následujícím kódem:

[!code-csharp[](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

Předchozí kód:

* Odebere kód, který se nepoužívá.
* Určuje, že databázi v paměti je vloženy do kontejneru služby.