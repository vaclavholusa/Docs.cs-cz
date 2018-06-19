---
title: Spravovat balíčky klienta s Bower v ASP.NET Core
author: rick-anderson
description: Spravovat balíčky klienta s Bower.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: 4f53d0f04d17631a12e2c2030d6dbb1f4fcc09d3
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838420"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Spravovat balíčky klienta s Bower v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel rýže](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), a [Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> A udržovat Bower jeho údržby programu doporučujeme používat jiné řešení. [Správce knihovny](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan pro zkrácení) je nový systém správy obsahu statické klienta Visual Studio. Yarn s Webpack je jeden oblíbenou alternativu, pro kterou [pokyny k migraci](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) jsou k dispozici.

[Bower](https://bower.io/) volá sám sebe "Správce balíčků pro web". V ekosystému .NET zaplňování void zanechaný NuGet neschopnost poskytovat statické soubory obsahu. Pro projekty ASP.NET Core, jsou tyto statické soubory systému na straně klienta knihovny jako [jQuery](http://jquery.com/) a [Bootstrap](http://getbootstrap.com/). Pro knihovny .NET, můžete dál používat [NuGet](https://www.nuget.org/) Správce balíčků.

Proces sestavení nové projekty vytvořené pomocí šablony projektů ASP.NET Core nastavení na straně klienta. [jQuery](http://jquery.com/) a [Bootstrap](http://getbootstrap.com/) jsou nainstalovány, a Bower je podporována.

Balíčky klienta jsou uvedeny v *bower.json* souboru. Šablony projektů ASP.NET Core nakonfiguruje *bower.json* s Bootstrap, jQuery a k ověřování jQuery.

V tomto kurzu přidáme podpora [písma Super](http://fontawesome.io). Bower balíčky můžete nainstalovat **spravovat balíčky Bower** uživatelského rozhraní nebo ručně v *bower.json* souboru.

### <a name="installation-via-manage-bower-packages-ui"></a>Instalaci přes spravovat balíčky Bower uživatelského rozhraní

* Vytvoření nové aplikace ASP.NET – webové jádro s **webové aplikace ASP.NET Core (.NET Core)** šablony. Vyberte **webovou aplikaci** a **bez ověřování**.

* Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a vyberte **spravovat balíčky Bower** (případně z hlavní nabídky, **projektu** > **spravovat balíčky Bower**).

* V **Bower: \<název projektu\>**  okně klikněte na kartu "Browse" a pak filtrovat seznam balíčků zadáním `font-awesome` do vyhledávacího pole:

  ![Správa balíčků bower](bower/_static/manage-bower-packages.png)

* Potvrďte, že "uložit změny do *bower.json*" je zaškrtnuté políčko. V rozevíracím seznamu vyberte verzi a klikněte na tlačítko **nainstalovat** tlačítko. **Výstup** okně se zobrazí podrobné informace o instalaci.

### <a name="manual-installation-in-bowerjson"></a>Ruční instalace v bower.json

Otevřete *bower.json* souboru a přidejte do závislosti "font Super". IntelliSense zobrazuje dostupné balíčky. Pokud je vybraný balíček, zobrazí se dostupné verze. Následující obrázky jsou starší a nebude odpovídat co vidíte.

![IntelliSense bower balíček Průzkumníka](bower/_static/add-package.png)

![bower verze IntelliSense](bower/_static/version-intelliSense.png)

Bower používá [sémantické verze](http://semver.org/) k uspořádání závislosti. Sémantické verze, také známé jako SemVer identifikuje balíčky s schématu číslování \<hlavní >.\< méně závažné >. \<oprava >. IntelliSense zjednodušuje tím, že zobrazuje pouze několik běžné volby sémantické verze. Horní položku v seznamu IntelliSense (4.6.3 v předchozím příkladu), je považován za nejnovější stabilní verze balíčku. Symbol šipka nahoru (^) odpovídá nejnovější hlavní verzi a znak tilda (~) odpovídá nejnovější dílčí verzi.

Uložit *bower.json* souboru. Visual Studio sleduje *bower.json* změny v souboru. Při ukládání, *bower instalace* se spustí příkaz. Najdete v okně výstupu **Bower nebo npm** zobrazení pro přesný příkaz spustit.

Otevřete *.bowerrc* souboru pod *bower.json*. `directory` Je nastavena na *wwwroot/lib* což označuje umístění Bower nainstaluje balíček prostředků.

```json
{
 "directory": "wwwroot/lib"
}
```

Do vyhledávacího pole v Průzkumníku řešení můžete použít k vyhledání a zobrazení písma Super balíčku.

Otevřete *Views\Shared\_Layout.cshtml* souboru a přidejte soubor písma Super šablon stylů CSS v prostředí [značky pomocná](xref:mvc/views/tag-helpers/intro) pro `Development`. V Průzkumníku řešení přetažení *písma awesome.css* uvnitř `<environment names="Development">` elementu.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

V produkční aplikace přidat *písma awesome.min.css* do pomocné rutiny prostředí značky pro `Staging,Production`.

Nahraďte obsah *Views\Home\About.cshtml* souboru nástroje Razor s následující kód:

[!code-html[](bower/sample/About.cshtml)]

Spusťte aplikaci a přejděte do zobrazení o ověření funguje písma Super balíčku.

## <a name="exploring-the-client-side-build-process"></a>Zkoumání procesu sestavení na straně klienta

Většina šablony projektů ASP.NET Core jsou již byla konfigurována pro použití Bower. Tento další návod začíná prázdný projekt ASP.NET Core a přidá každého jednotlivého ručně, abyste získali chování pro použití Bower v projektu. Můžete zobrazit, co se stane s strukturu projektu a runtime výstup jako každé změně konfigurace.

Obecné kroky pro použití s Bower procesu sestavení na straně klienta jsou:

* Definujte balíčky použité ve vašem projektu. <!-- once defined, you don't need to download them, VS does -->
* Odkaz na balíčky z webových stránek.

### <a name="define-packages"></a>Definování balíčků

Jakmile seznam balíčků v *bower.json* je se stažení souboru, Visual Studio. Následující příklad používá k načtení knihovny jQuery a Bootstrap na Bower *wwwroot* složky.

* Vytvoření nové aplikace ASP.NET – webové jádro s **webové aplikace ASP.NET Core (.NET Core)** šablony. Vyberte **prázdný** šablona projektu a klikněte na tlačítko **OK**.

* V Průzkumníku řešení klikněte pravým tlačítkem na projekt > **přidat novou položku** a vyberte **Bower konfigurační soubor**. Poznámka: A *.bowerrc* soubor je taky přidaný.

* Otevřete *bower.json*a přidejte jquery a bootstrap na `dependencies` oddílu. Výsledná *bower.json* soubor bude vypadat jako v následujícím příkladu. Verze bude časem změnit a nemusí odpovídat následující obrázek.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Uložit *bower.json* souboru.

  Ověřte, tento projekt zahrnuje *bootstrap* a *jQuery* adresářů v *wwwroot/lib*. Bower používá *.bowerrc* instalace prostředků v souboru *wwwroot/lib*.

  Poznámka: Rozhraní "Správa balíčků Bower" poskytuje alternativu k ruční souboru úpravy.

### <a name="enable-static-files"></a>Povolte statické soubory

* Přidat `Microsoft.AspNetCore.StaticFiles` balíček NuGet do projektu.
* Povolte statické soubory ke zpracování s [middleware se statickými soubory](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Přidejte volání [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) k `Configure` metodu `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Referenčních balíčků

V této části vytvoříte stránky HTML a ověří, zda má přístup k nasazených balíčků.

* Přidat novou stránku HTML s názvem *Index.html* k *wwwroot* složky. Poznámka: Je nutné přidat soubor HTML *wwwroot* složky. Ve výchozím nastavení, nelze zpracovat statický obsah mimo *wwwroot*. V tématu [statické soubory](xref:fundamentals/static-files) Další informace.

  Nahraďte obsah *Index.html* s následující kód:

[!code-html[](bower/sample/Index.html)]

* Spusťte aplikaci a přejděte do `http://localhost:<port>/Index.html`. Alternativně s *Index.html* otevřené, stiskněte `Ctrl+Shift+W`. Ověřte, že se použije jumbotron stylů, kód jazyka jQuery odpovídá při kliknutí na tlačítko a že zavedení tlačítko se změní stav.

  ![Styl jumbotron použitý](bower/_static/jumbotron.png)
