## <a name="register-the-database-context"></a><span data-ttu-id="3665f-101">Zaregistrovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="3665f-101">Register the database context</span></span>

<span data-ttu-id="3665f-102">V tomto kroku není zaregistrována kontext databáze [vkládání závislostí](xref:fundamentals/dependency-injection) kontejneru.</span><span class="sxs-lookup"><span data-stu-id="3665f-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3665f-103">Služby (například kontext databáze), které jsou registrovány kontejneru pro vkládání (DI) závislosti jsou dostupné v řadičích.</span><span class="sxs-lookup"><span data-stu-id="3665f-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="3665f-104">Zaregistrovat kontext databáze kontejneru služby pomocí integrovanou podporu pro [vkládání závislostí](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3665f-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3665f-105">Nahraďte obsah *Startup.cs* soubor s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="3665f-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]

::: moniker-end  

<span data-ttu-id="3665f-106">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="3665f-106">The preceding code:</span></span>

* <span data-ttu-id="3665f-107">Odebere kód nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="3665f-107">Removes the unused code.</span></span>
* <span data-ttu-id="3665f-108">Určuje, že databázi v paměti je vloženy do kontejneru služby.</span><span class="sxs-lookup"><span data-stu-id="3665f-108">Specifies an in-memory database is injected into the service container.</span></span>
