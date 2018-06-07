---
title: Přidat, stahování a odstraňování dat vlastní uživatele k identitě v projektu ASP.NET Core
author: rick-anderson
description: Zjistěte, jak přidat vlastní uživatelská data do Identity v projektu ASP.NET Core. Odstraníte data za GDPR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: cc7b29499e9db702cab70be7c15eac53373d450d
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819068"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Přidat, stahování a odstraňování dat vlastní uživatele k identitě v projektu ASP.NET Core

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento článek ukazuje, jak:

* Přidáte vlastní uživatelská data do webové aplikace ASP.NET Core.
* Uspořádání vlastního uživatelského datového modelu s [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atributu, je automaticky dostupný pro stažení a odstranění. Nastavení dat na moct stáhnout a odstranit pomáhá splňují [GDPR](xref:security/gdpr) požadavky.

Ukázkový projekt je vytvořený z stránky Razor webové aplikace, ale tyto pokyny jsou podobné pro webovou aplikaci ASP.NET MVC jádra.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Vytvoření webové aplikace Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**. Název projektu **WebApp1** Pokud budete chtít odpovídat obor názvů [stažení ukázky](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kódu.
* Vyberte **webové aplikace ASP.NET Core** > **OK**
* Vyberte **ASP.NET Core 2.1** v rozevírací nabídce
* Vyberte **webovou aplikaci**  > **OK**
* Sestavte a spusťte projekt.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

------

## <a name="run-the-identity-scaffolder"></a>Spustit Identity scaffolder

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt > **přidat** > **novou vygenerovanou položku**.
* V levém podokně nástroje **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **Identity** > **přidat**.
* V **přidat Identity** dialogové okno, následující možnosti:
  * Vyberte existující soubor rozložení *~/Pages/Shared/_Layout.cshtml*
  * Vyberte následující soubory k přepsání:
    * **Účet nebo registrace**
    * **Účet nebo spravovat/indexu**
  * Vyberte **+** tlačítko pro vytvoření nového **třída kontextu dat**. Přijměte typ (**WebApp1.Models.WebApp1Context** Pokud název projektu **WebApp1**).
  * Vyberte **+** tlačítko pro vytvoření nového **třídu uživatelů**. Přijměte typ (**WebApp1User** Pokud název projektu **WebApp1**) > **přidat**.
* Vyberte **přidat**.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Pokud jste nenainstalovali dříve ASP.NET scaffolder, nainstalujte ji:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Přidat odkaz na balíček [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) k souboru projektu (.csproj). Spusťte následující příkaz v adresáři projektu:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Spusťte následující příkaz, který zobrazí seznam možností scaffolder Identity:

```cli
dotnet aspnet-codegenerator identity -h
```

Ve složce projektu spusťte Identity scaffolder:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Postupujte podle pokynů v [UseAuthentication, migrace a rozložení](xref:security/authentication/scaffold-identity#efm) provést následující kroky:

* Vytvořte migrace a aktualizaci databáze.
* Add `UseAuthentication` to `Startup.Configure`.
* Přidat `<partial name="_LoginPartial" />` rozložení souboru.
* Testovací aplikace:
  * Registrace uživatele
  * Vyberte nové uživatelské jméno (vedle **odhlášení** odkaz). Možná budete muset rozbalte okno nebo vyberte ikony navigačního panelu zobrazit uživatelské jméno a další odkazy.
  * Vyberte **osobní Data** kartě.
  * Vyberte **Stáhnout** tlačítko a vyšetřit *PersonalData.json* souboru.
  * Testovací **odstranit** tlačítko, které odstraní přihlášeného uživatele.

## <a name="add-custom-user-data-to-the-identity-db"></a>Přidat vlastní uživatelská data do databáze Identity

Aktualizace `IdentityUser` odvozené třídy s vlastní vlastnosti. Pokud jste projekt WebApp1, je soubor s názvem *Areas/Identity/Data/WebApp1User.cs*. Aktualizujte soubor s následujícím kódem:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Vlastnosti označených pomocí [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) atribut jsou:

* Odstranit, když *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* stránky Razor volá `UserManager.Delete`.
* Součástí stažená data pomocí *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* stránky Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Account/Manage/Index.cshtml stránku aktualizace

Aktualizace `InputModel` v *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* s následujícími službami zvýrazněná kódu:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95)]

Aktualizace *Areas/Identity/Pages/Account/Manage/Index.cshtml* s následující zvýrazněný kód:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Account/Register.cshtml stránku aktualizace

Aktualizace `InputModel` v *Areas/Identity/Pages/Account/Register.cshtml.cs* s následujícími službami zvýrazněná kódu:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Aktualizace *Areas/Identity/Pages/Account/Register.cshtml* s následující zvýrazněný kód:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Sestavte projekt.

### <a name="add-a-migration-for-the-custom-user-data"></a>Přidat migrace pro vlastní data

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

V sadě Visual Studio **Konzola správce balíčků**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Test vytvořit, zobrazit, stáhnout, odstranit vlastní uživatelská data

Testovací aplikace:

* Registrace nového uživatele.
* Zobrazení dat vlastní uživatele na `/Identity/Account/Manage` stránky.
* Stáhnout a zobrazit osobní údaje uživatelů z `/Identity/Account/Manage/PersonalData` stránky.
