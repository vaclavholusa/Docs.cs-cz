Spusťte generátor Identity:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.
* V levém podokně **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte **Identity** > **přidat**.
* V **ADD Identity** dialogového okna, vyberte požadované možnosti.
  * Vyberte existující stránku rozložení nebo rozložení souboru budou přepsány nesprávný kód. Při výběru existujícího souboru _Layout.cshtml je **není** přepsán.

 Například `~/Pages/Shared/_Layout.cshtml` pro stránky Razor `~/Views/Shared/_Layout.cshtml` pro projekty MVC
* Pokud chcete použít existující kontext dat, vyberte aspoň jeden soubor přepsat. Musíte vybrat aspoň jeden soubor a přidejte datový kontext.
  * Vyberte vaše třída kontextu dat.
  * Vyberte **přidat**.
* Vytvoří nový kontext uživatele a případně vytvořit třídu vlastního uživatele pro identitu:
  * Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.
  * Vyberte **přidat**.

Poznámka: Pokud vytváříte nový kontext uživatele, není nutné vybrat soubor, který chcete přepsat.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Pokud jste nenainstalovali dříve generátor technologie ASP.NET, nainstalujte ho:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Přidat odkaz na balíček pro [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) souboru. Spusťte následující příkaz v adresáři projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Spusťte následující příkaz k výpisu možností generátor Identity:

```cli
dotnet aspnet-codegenerator identity -h
```

Ve složce projektu spusťte s možnostmi, které chcete generátor Identity. Například nastavit identitu pomocí výchozího uživatelského rozhraní a minimální počet souborů, spusťte následující příkaz. Použijte správný plně kvalifikovaný název pro váš kontext databáze:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
