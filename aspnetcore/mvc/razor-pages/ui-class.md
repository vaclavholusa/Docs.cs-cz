---
title: Opakovaně použitelné uživatelské rozhraní Razor v knihovny tříd ASP.NET Core
author: Rick-Anderson
description: Vysvětluje, jak vytvářet opakovaně použitelné uživatelské rozhraní Razor v knihovně tříd.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Vytvořte opakovaně použitelné uživatelské rozhraní v projektu knihovny tříd Razor ASP.NET Core.

podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Zobrazení syntaxe Razor, stránky, řadiče, modely stránky a datové modely se dají vytvářet do třídy Library(RCL) syntaxe Razor. RCL může být zabalené a znovu použít. Aplikace může obsahovat RCL a přepsání, zobrazení a stránky, které obsahuje. Když najde zobrazení, částečná zobrazení nebo stránky Razor ve webové aplikaci a RCL, kód Razor (*.cshtml* souboru) ve webové aplikaci, dostane přednost.

Tato funkce vyžaduje [!INCLUDE[](~/includes/2.1-SDK.md)]

[!INCLUDE[](~/includes/2.1.md)]

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Vytvoření knihovny tříd obsahující Razor uživatelského rozhraní

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vyberte **webové aplikace ASP.NET Core**.
* Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.
* Vyberte **knihovny tříd Razor** > **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Z příkazového řádku, spusťte `dotnet new razorclasslib`. Příklad:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Další informace najdete v tématu [dotnet nové](/dotnet/core/tools/dotnet-new).

------
Přidáte soubory Razor RCL.

Doporučujeme, abyste RCL obsahu přejděte v *oblasti* složky. 


## <a name="referencing-razor-class-library-content"></a>Odkazování na obsah knihovny Razor – třída

RCL lze odkazovat pomocí:

* Balíček NuGet. V tématu [balíčky NuGet vytváření](/nuget/create-packages/creating-a-package) a [dotnet. Přidejte balíček](/dotnet/core/tools/dotnet-add-package) a [vytvoření a publikování balíčku NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{Název projektu} .csproj*. V tématu [dotnet – přidat odkaz](/dotnet/core/tools/dotnet-add-reference).

### <a name="partial-files-access-in-the-rcl"></a>Částečné soubory přístup v RCL

Pro obsah mimo RCL runtime ASP.NET Core nehledá částečné soubory v RCL.

Například v ukázkové stahování *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení může **není** bude odkazovat ve *WebApp1\Pages\About.cshtml* . Nicméně stránky v RCL ( *RazorUIClassLib /* **můžete** přístup *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Návod: Vytvoření projektu knihovny tříd Razor a použít z projektu stránky Razor

Si můžete stáhnout [dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) a otestovat ji spíš než její vytvoření. Stažení ukázkové obsahuje další kód a odkazy, které usnadňují testování projektu. Můžete ponechat zpětné vazby v [potíže Githubu](https://github.com/aspnet/Docs/issues/6098) s komentáře na stažení ukázky a podrobné pokyny.

### <a name="test-the-download-app"></a>Testování stažení aplikace

Pokud nebyly staženy dokončené aplikace a by místo vytvoření projektu návod, pokračujte [další části](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otevřete *.sln* souborů v sadě Visual Studio. Spusťte aplikaci.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Z příkazového řádku v *rozhraní příkazového řádku* directory sestavení RCL a webové aplikace.

``` CLI
dotnet build
```

Přesunout do *WebApp1* adresáře a spusťte aplikaci:

``` CLI
dotnet run
```
------

Postupujte podle pokynů v [WebApp1 testu](#test)

## <a name="create-a-razor-class-library"></a>Vytvoření knihovny tříd Razor

V této části se vytvoří Razor třídu knihovny (RCL). RCL jsou přidány soubory Razor.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Vytvoření projektu RCL:

* Ze sady Visual Studio **soubor** nabídce vyberte možnost **nový** > **projektu**.
* Vyberte **webové aplikace ASP.NET Core**.
* Název aplikace **RazorUIClassLib**.
* Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.
* Vyberte **knihovny tříd Razor** > **OK**.

Vytvoření stránky Razor webové aplikace:

* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na řešení > **přidat** >  **nový projekt**.
* Vyberte **webové aplikace ASP.NET Core**.
* Název aplikace **WebApp1**.
* Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.
* Vyberte **webovou aplikaci** > **OK**.

### <a name="add-razor-files-and-folders-to-the-project"></a>Přidejte Razor soubory a složky, do projektu.

* Přidání souboru nástroje Razor částečné zobrazení s názvem *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.
* Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Kopírování *soubor _ViewStart.cshtml* soubor z projekt WebApp1 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.

  [Viewstart](xref:mvc/views/layout#running-code-before-each-view) soubor je vyžadován pro použití rozložení stránky Razor projektu.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Z příkazového řádku spusťte následující příkaz:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Předchozí příkazy:

* Vytvoří `RazorUIClassLib` knihovny tříd Razor (RCL).
* Vytvoří stránku _Message Razor a přidává ji k RCL. `-np` Parametr vytvoří stránku bez `PageModel`.
* Vytvoří [viewstart](xref:mvc/views/layout#running-code-before-each-view) souboru a přidá ho RCL.

Soubor viewstart je potřeba použít rozložení stránky Razor projektu (která je přidána v další části).

Aktualizace stránky Razor:

* Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* následujícím kódem:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` je potřeba použít částečné zobrazení (`<partial name="_Message" />`). Místo včetně `@addTagHelper` direktivy, můžete přidat *_ViewImports.cshtml* souboru. Příklad:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Další informace o viewimports najdete v tématu [import sdílených direktivy](xref:mvc/views/layout#importing-shared-directives)

* Sestavení knihovny tříd se ověřit, zda že nejsou žádné chyby kompilátoru:

``` CLI
dotnet build RazorUIClassLib
```

Výstup sestavení obsahuje *RazorUIClassLib.dll* a *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* obsahuje kompilovaný obsah Razor.

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Použití knihovny uživatelského rozhraní Razor z projektu stránky Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **nastavit jako spouštěný projekt**.
* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **sestavení závislosti** > **závislosti projektu**.
* Zkontrolujte **RazorUIClassLib** jako závislost **WebApp1**.
* Z **Průzkumníku řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **přidat** > **odkaz**.
* V **správce odkazů** dialogové okno, zkontrolujte **RazorUIClassLib** > **OK**.

Spusťte aplikaci.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Vytvoření stránky Razor webové aplikace a řešení obsahující stránky Razor aplikace a knihovny tříd Razor:

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Sestavení a spuštění webové aplikace:

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a>Test WebApp1

Ověřte, zda že je používán knihovny tříd Razor uživatelského rozhraní.

* Přejděte do `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Přepsání, zobrazení, částečná zobrazení a stránky

Když najde zobrazení, částečná zobrazení nebo stránky Razor ve webové aplikace a knihovny tříd Razor, kód Razor (*.cshtml* souboru) ve webové aplikaci, dostane přednost. Například přidejte *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* k WebApp1, a Page1 v WebApp1 má přednost Page1in knihovny tříd Razor.

Při stahování ukázka přejmenovat *WebApp1, oblasti nebo MyFeature2* k *WebApp1, oblasti nebo MyFeature* k testování přednost před.

Kopírování *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení k *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Aktualizujte kód označující nové umístění. Sestavení a spuštění aplikace k ověření, že verze aplikace s částečným je používán.
