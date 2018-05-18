<span data-ttu-id="660db-101">Spusťte Identity scaffolder:</span><span class="sxs-lookup"><span data-stu-id="660db-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="660db-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="660db-102">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="660db-103">Z **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="660db-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="660db-104">V levém podokně nástroje **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **Identity** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="660db-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="660db-105">V **přidat Identity** dialogovém okně, vyberte požadované možnosti.</span><span class="sxs-lookup"><span data-stu-id="660db-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="660db-106">Vyberte existující stránku rozložení a rozložení souboru, budou přepsány nesprávný kód.</span><span class="sxs-lookup"><span data-stu-id="660db-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="660db-107">Pokud je vybrána existující soubor _Layout.cshtml, je **není** přepsat.</span><span class="sxs-lookup"><span data-stu-id="660db-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="660db-108">Například `~/Pages/Shared/_Layout.cshtml` pro stránky Razor `~/Views/Shared/_Layout.cshtml` pro projekty MVC</span><span class="sxs-lookup"><span data-stu-id="660db-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span> 
* <span data-ttu-id="660db-109">Pokud chcete používat vaše stávající kontextu dat, vyberte alespoň jeden soubor k přepsání.</span><span class="sxs-lookup"><span data-stu-id="660db-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="660db-110">Je nutné vybrat alespoň jeden soubor přidat vaše data kontextu.</span><span class="sxs-lookup"><span data-stu-id="660db-110">You must select at least one file to add your data context.</span></span> 
  * <span data-ttu-id="660db-111">Vyberte třídu kontextu vaše data.</span><span class="sxs-lookup"><span data-stu-id="660db-111">Select your data context class.</span></span>
  * <span data-ttu-id="660db-112">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="660db-112">Select **ADD**.</span></span>
* <span data-ttu-id="660db-113">Vytvoří nový kontext uživatele a případně vytvořit vlastní uživatelská třídu pro Identity:</span><span class="sxs-lookup"><span data-stu-id="660db-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="660db-114">Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="660db-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="660db-115">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="660db-115">Select **ADD**.</span></span>
  
<span data-ttu-id="660db-116">Poznámka: Pokud vytváříte nový kontext uživatel, nemáte a vyberte soubor, který chcete přepsat.</span><span class="sxs-lookup"><span data-stu-id="660db-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="660db-117">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="660db-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="660db-118">Pokud jste nenainstalovali dříve ASP.NET scaffolder, nainstalujte ji:</span><span class="sxs-lookup"><span data-stu-id="660db-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator --version 2.1.0-rc1-final
```

<span data-ttu-id="660db-119">Přidat odkaz na balíček [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) souboru.</span><span class="sxs-lookup"><span data-stu-id="660db-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="660db-120">Spusťte následující příkaz v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="660db-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
```

<span data-ttu-id="660db-121">Spusťte následující příkaz, který zobrazí seznam možností scaffolder Identity:</span><span class="sxs-lookup"><span data-stu-id="660db-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="660db-122">Ve složce projektu spusťte Identity scaffolder s možností, které chcete.</span><span class="sxs-lookup"><span data-stu-id="660db-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="660db-123">Například nastavit identity s výchozím uživatelského rozhraní a minimální počet souborů, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="660db-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="660db-124">Použijte správný plně kvalifikovaný název vaší kontext databáze:</span><span class="sxs-lookup"><span data-stu-id="660db-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```
-------------
