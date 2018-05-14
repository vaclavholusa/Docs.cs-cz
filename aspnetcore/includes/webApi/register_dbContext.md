## <a name="register-the-database-context"></a>Zaregistrovat kontext databáze

V tomto kroku není zaregistrována kontext databáze [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru. Služby (například kontext databáze), které jsou registrovány kontejneru pro vkládání (DI) závislosti jsou dostupné v řadičích.

Zaregistrovat kontext databáze kontejneru služby pomocí integrovanou podporu pro [vkládání závislostí](xref:fundamentals/dependency-injection). Nahraďte obsah *Startup.cs* soubor s následujícím kódem:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]
::: moniker-end

Předchozí kód:

* Odebere kód nepoužívá.
* Určuje, že databázi v paměti je vloženy do kontejneru služby.