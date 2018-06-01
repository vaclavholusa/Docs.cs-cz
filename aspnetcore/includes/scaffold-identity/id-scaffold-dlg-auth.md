Spusťte Identity scaffolder:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.
* V levém podokně nástroje **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **Identity** > **přidat**.
* V **přidat Identity** dialogovém okně, vyberte požadované možnosti.
  * Vyberte existující stránku rozložení a rozložení souboru, budou přepsány nesprávný kód. Pokud je vybrána existující soubor _Layout.cshtml, je **není** přepsat.

 Například `~/Pages/Shared/_Layout.cshtml` pro stránky Razor `~/Views/Shared/_Layout.cshtml` pro projekty MVC 
* Pokud chcete používat vaše stávající kontextu dat, vyberte alespoň jeden soubor k přepsání. Je nutné vybrat alespoň jeden soubor přidat vaše data kontextu. 
  * Vyberte třídu kontextu vaše data.
  * Vyberte **přidat**.
* Vytvoří nový kontext uživatele a případně vytvořit vlastní uživatelská třídu pro Identity:
  * Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.
  * Vyberte **přidat**.
  
Poznámka: Pokud vytváříte nový kontext uživatel, nemáte a vyberte soubor, který chcete přepsat.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Pokud jste nenainstalovali dříve ASP.NET scaffolder, nainstalujte ji:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Přidat odkaz na balíček [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) souboru. Spusťte následující příkaz v adresáři projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
```

Spusťte následující příkaz, který zobrazí seznam možností scaffolder Identity:

```cli
dotnet aspnet-codegenerator identity -h
```

Ve složce projektu spusťte Identity scaffolder s možností, které chcete. Například nastavit identity s výchozím uživatelského rozhraní a minimální počet souborů, spusťte následující příkaz. Použijte správný plně kvalifikovaný název vaší kontext databáze:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```
-------------
