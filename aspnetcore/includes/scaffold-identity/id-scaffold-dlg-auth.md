<span data-ttu-id="63fed-101">Spusťte generátor Identity:</span><span class="sxs-lookup"><span data-stu-id="63fed-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="63fed-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="63fed-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="63fed-103">Z **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="63fed-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="63fed-104">V levém podokně **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte **Identity** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="63fed-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="63fed-105">V **ADD Identity** dialogového okna, vyberte požadované možnosti.</span><span class="sxs-lookup"><span data-stu-id="63fed-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="63fed-106">Vyberte existující stránku rozložení nebo rozložení souboru budou přepsány nesprávný kód.</span><span class="sxs-lookup"><span data-stu-id="63fed-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="63fed-107">Při výběru existujícího souboru _Layout.cshtml je **není** přepsán.</span><span class="sxs-lookup"><span data-stu-id="63fed-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="63fed-108">Například `~/Pages/Shared/_Layout.cshtml` pro stránky Razor `~/Views/Shared/_Layout.cshtml` pro projekty MVC</span><span class="sxs-lookup"><span data-stu-id="63fed-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="63fed-109">Pokud chcete použít existující kontext dat, vyberte aspoň jeden soubor přepsat.</span><span class="sxs-lookup"><span data-stu-id="63fed-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="63fed-110">Musíte vybrat aspoň jeden soubor a přidejte datový kontext.</span><span class="sxs-lookup"><span data-stu-id="63fed-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="63fed-111">Vyberte vaše třída kontextu dat.</span><span class="sxs-lookup"><span data-stu-id="63fed-111">Select your data context class.</span></span>
  * <span data-ttu-id="63fed-112">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="63fed-112">Select **ADD**.</span></span>
* <span data-ttu-id="63fed-113">Vytvoří nový kontext uživatele a případně vytvořit třídu vlastního uživatele pro identitu:</span><span class="sxs-lookup"><span data-stu-id="63fed-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="63fed-114">Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.</span><span class="sxs-lookup"><span data-stu-id="63fed-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="63fed-115">Vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="63fed-115">Select **ADD**.</span></span>

<span data-ttu-id="63fed-116">Poznámka: Pokud vytváříte nový kontext uživatele, není nutné vybrat soubor, který chcete přepsat.</span><span class="sxs-lookup"><span data-stu-id="63fed-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="63fed-117">Rozhraní příkazového řádku .NET Core</span><span class="sxs-lookup"><span data-stu-id="63fed-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="63fed-118">Pokud jste nenainstalovali dříve generátor technologie ASP.NET, nainstalujte ho:</span><span class="sxs-lookup"><span data-stu-id="63fed-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="63fed-119">Přidat odkaz na balíček pro [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) souboru.</span><span class="sxs-lookup"><span data-stu-id="63fed-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="63fed-120">Spusťte následující příkaz v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="63fed-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="63fed-121">Spusťte následující příkaz k výpisu možností generátor Identity:</span><span class="sxs-lookup"><span data-stu-id="63fed-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="63fed-122">Ve složce projektu spusťte s možnostmi, které chcete generátor Identity.</span><span class="sxs-lookup"><span data-stu-id="63fed-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="63fed-123">Například nastavit identitu pomocí výchozího uživatelského rozhraní a minimální počet souborů, spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="63fed-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="63fed-124">Použijte správný plně kvalifikovaný název pro váš kontext databáze:</span><span class="sxs-lookup"><span data-stu-id="63fed-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="63fed-125">PowerShell používá jako oddělovač příkazu středník.</span><span class="sxs-lookup"><span data-stu-id="63fed-125">Powershell uses semicolon as a command separator.</span></span> <span data-ttu-id="63fed-126">Při použití prostředí powershell, řídicí středníky v seznamu souborů nebo vložit seznam souborů v dvojitých uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="63fed-126">When using powershell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="63fed-127">Příklad:</span><span class="sxs-lookup"><span data-stu-id="63fed-127">For example:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```
-------------
