Spusťte Identity scaffolder:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.
* V levém podokně nástroje **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **Identity** > **přidat**.
* V **přidat Identity** dialogovém okně, vyberte požadované možnosti.
  * Vyberte existující stránku rozložení a rozložení souboru, budou přepsány nesprávný kód. Například `~/Pages/Shared/_Layout.cshtml` pro stránky Razor `~/Views/Shared/_Layout.cshtml` pro projekty MVC 
  * Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**.
* Vyberte **přidat**.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Pokud jste nenainstalovali dříve ASP.NET scaffolder, nainstalujte ji:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator --version 2.1.0-rc1-final
```

Přidat odkaz na balíček [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) do projektu (\*.csproj) souboru. Spusťte následující příkaz v adresáři projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v "2.1.0-rc1-final"
dotnet restore
```

Spusťte následující příkaz, který zobrazí seznam možností scaffolder Identity:


```cli
dotnet aspnet-codegenerator identity -h
```

Ve složce projektu spusťte Identity scaffolder s možností, které chcete. Například nastavit identity s výchozím uživatelského rozhraní a minimální počet souborů, spusťte následující příkaz:

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```
-------------