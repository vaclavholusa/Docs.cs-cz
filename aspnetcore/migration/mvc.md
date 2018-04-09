---
title: Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC
author: ardalis
description: Zjistěte, jak začít pracovat migrace projektu aplikace ASP.NET MVC do architektury ASP.NET MVC jádra.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: e249be06726b307a1c41a525a132f7e0ab8b50ee
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), a [Scott Addie](https://scottaddie.com)

Tento článek ukazuje, jak začít s migrací do projektu aplikace ASP.NET MVC [ASP.NET MVC základní](../mvc/overview.md). V procesu se označuje mnoho věcí, které se změnily z rozhraní ASP.NET MVC. Migrace z rozhraní ASP.NET MVC je více proces krok a tento článek se zabývá počáteční nastavení, základní řadiče a zobrazení, statický obsah a závislosti na straně klienta. Další články zahrnovat migraci konfigurace a kód identit v mnoha projekty ASP.NET MVC nalezen.

> [!NOTE]
> Čísla verzí v ukázkách nemusí být aktuální. Musíte aktualizovat projekty, odpovídajícím způsobem.

## <a name="create-the-starter-aspnet-mvc-project"></a>Vytvořit úvodní projektu ASP.NET MVC

K předvedení upgradu, začneme vytvořením aplikace ASP.NET MVC. Vytvořit s názvem *WebApp1* , obor názvů bude shodovat s projektu ASP.NET Core vytvoříme v dalším kroku.

![Dialogové okno Visual Studio nový projekt](mvc/_static/new-project.png)

![Dialogové okno nové webové aplikace: projektu šablony MVC vybrali panelu šablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Volitelné:* změnit název řešení od *WebApp1* k *Mvc5*. Visual Studio se zobrazí název nového řešení (*Mvc5*), které usnadní říct tento projekt z projektu další.

## <a name="create-the-aspnet-core-project"></a>Vytvoření projektu ASP.NET Core

Vytvořte novou *prázdný* ASP.NET Core webové aplikace se stejným názvem jako předchozí projekt (*WebApp1*), obory názvů v dva projekty shodovat. S o stejný obor názvů usnadňuje zkopírujte kód mezi dva projekty. Budete mít k vytvoření tohoto projektu v jiném adresáři než předchozí projekt, který používá stejný název.

![Dialogové okno Nový projekt](mvc/_static/new_core.png)

![Dialogové okno nové webové aplikace ASP.NET: prázdná šablona projektu vybrané panelu ASP.NET Core šablony](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Volitelné:* vytvoření nové aplikace ASP.NET Core pomocí *webové aplikace* šablona projektu. Název projektu *WebApp1*a vyberte některou možnost ověřování z **jednotlivé uživatelské účty**. Přejmenujte tuto aplikaci a *FullAspNetCore*. Vytvoření projektu budou ušetřit čas v převodu. Můžete si prohlédnout kód generovaný šablony najdete v části konečný výsledek nebo zkopírujte kód do projektu převod. Je také užitečné, pokud zablokuje v kroku převod k porovnání s projektem šablona vytvořena.

## <a name="configure-the-site-to-use-mvc"></a>Konfigurace lokality k použití MVC

* Nainstalujte `Microsoft.AspNetCore.Mvc` a `Microsoft.AspNetCore.StaticFiles` balíčky NuGet.

  `Microsoft.AspNetCore.Mvc` je rozhraní ASP.NET MVC jádra. `Microsoft.AspNetCore.StaticFiles` je obslužná rutina statických souborů. Modulem runtime ASP.NET je modulární a musí explicitně přihlášení poskytovat statické soubory (viz [pracovat s statické soubory](../fundamentals/static-files.md)).

* Otevřete *.csproj* souboru (klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **upravit WebApp1.csproj**) a přidejte `PrepareForPublish` cíl:

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  `PrepareForPublish` Cíl je nutný k získání klientské knihovny prostřednictvím Bower. Budeme mluvit o který později.

* Otevřete *Startup.cs* souboru a změnit kód tak, aby odpovídala následující:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  `UseStaticFiles` Metoda rozšíření přidá obslužné rutiny statických souborů. Jak je uvedeno nahoře, modulem runtime ASP.NET je modulární a musí explicitně přihlášení poskytovat statické soubory. `UseMvc` Přidá metody rozšíření směrování. Další informace najdete v tématu [spuštění aplikace](../fundamentals/startup.md) a [směrování](../fundamentals/routing.md).

## <a name="add-a-controller-and-view"></a>Přidání kontroleru a zobrazení

V této části přidáte minimální řadiče a zobrazení, která bude sloužit jako zástupné symboly ASP.NET MVC jsou řadič MVC a zobrazení, že budete migrovat v další části.

* Přidat *řadiče* složky.

* Přidat **třídy kontroleru MVC** s názvem *HomeController.cs* k *řadiče* složky.

![Přidat novou položku – dialogové okno](mvc/_static/add_mvc_ctl.png)

* Přidat *zobrazení* složky.

* Přidat *zobrazení Domů* složky.

* Přidat *Index.cshtml* stránka zobrazení MVC do *zobrazení Domů* složky.

![Přidat novou položku – dialogové okno](mvc/_static/view.png)

Struktura projektu je zobrazena níže:

![Průzkumník řešení zobrazující soubory a složky WebApp1](mvc/_static/project-structure-controller-view.png)

Nahraďte obsah *Views/Home/Index.cshtml* soubor s následující:

```html
<h1>Hello world!</h1>
```

Spusťte aplikaci.

![Webové aplikace, otevřete v Microsoft Edge](mvc/_static/hello-world.png)

V tématu [řadiče](xref:mvc/controllers/actions) a [zobrazení](xref:mvc/views/overview) Další informace.

Teď, když máme minimální funkční projekt ASP.NET Core, můžeme začít migrace funkce z projektu ASP.NET MVC. Budeme muset přesunout následující:

* obsah na straně klienta (šablon stylů CSS, písma a skripty)

* kontrolery

* zobrazení

* modely

* Sdružování

* filtry

* Přihlaste se vstup/výstup identity (to bude provedeno v dalším kurzu.)

## <a name="controllers-and-views"></a>Kontrolery a zobrazení

* Zkopírujte všechny metody z rozhraní ASP.NET MVC `HomeController` do nového `HomeController`. Upozorňujeme, že v architektuře ASP.NET MVC předdefinované šablony řadiče akce metoda návratový typ je [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); v aplikaci ASP.NET MVC jádra, metody akce návratový `IActionResult` místo. `ActionResult` implementuje `IActionResult`, takže není nutné změnit návratový typ vaší metody akce.

* Kopírování *About.cshtml*, *Contact.cshtml*, a *Index.cshtml* Razor zobrazit soubory z projektu ASP.NET MVC do projektu ASP.NET Core.

* Spuštění aplikace ASP.NET Core a testování jednotlivých metod. Jsme jste nemigrovali rozložení souboru nebo styly ještě, takže vykreslené zobrazení bude obsahovat pouze obsah souborů zobrazení. Nebudete mít rozložení souboru vygenerovaného odkazy `About` a `Contact` zobrazení, takže budete muset je vyvolat z prohlížeče (Nahraďte **4492** číslem portu, na které se používají ve vašem projektu).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kontaktujte stránky](mvc/_static/contact-page.png)

Poznámka: nedostatek položky stylů a nabídky. To jsme budete opravíme v další části.

## <a name="static-content"></a>Statický obsah

V předchozích verzích rozhraní ASP.NET MVC statický obsah hostitelem byl z kořenového adresáře webového projektu a byl smíšeného s soubory na straně serveru. V ASP.NET Core, statický obsah uložený v *wwwroot* složky. Budete chtít zkopírujte statický obsah z původního aplikaci ASP.NET MVC *wwwroot* složku ve vašem projektu ASP.NET Core. V této ukázkové převod:

* Kopírování *favicon.ico* soubor z původní projekt MVC *wwwroot* složky v projektu ASP.NET Core.

Starý ASP.NET MVC projektu používá [Bootstrap](http://getbootstrap.com/) pro jeho stylů a úložišť, službou Bootstrap nástroje soubory *obsahu* a *skripty* složek. Šablony, která generuje staré projektu ASP.NET MVC, odkazuje na Bootstrap v rozložení souboru (*Views/Shared/_Layout.cshtml*). Může zkopírovat *bootstrap.js* a *bootstrap.css* soubory z rozhraní ASP.NET MVC projektu do *wwwroot* nepoužívá složky v novém projektu, ale tento přístup Vylepšené mechanismus pro správu klienta závislostí v ASP.NET Core.

V novém projektu, přidáme podporu pro Bootstrap (a další klientské knihovny) pomocí [Bower](https://bower.io/):

* Přidat [Bower](https://bower.io/) konfigurační soubor s názvem *bower.json* do kořenového adresáře projektu (klikněte pravým tlačítkem na projekt a potom **Přidat > novou položku > Bower konfigurační soubor**). Přidat [Bootstrap](http://getbootstrap.com/) a [jQuery](https://jquery.com/) do souboru (viz níže zvýrazněné řádky).

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

Při ukládání souboru, Bower automaticky stáhnout závislosti na *wwwroot/lib* složky. Můžete použít **Průzkumník služby Search řešení** pole najít cestu prostředky:

![prostředky jQuery zobrazený ve výsledcích hledání Průzkumníku řešení](mvc/_static/search.png)

V tématu [spravovat klientské balíčky s Bower](../client-side/bower.md) Další informace.

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>Migrace na soubor rozložení

* Kopírování *soubor _ViewStart.cshtml* soubor z původního projektu ASP.NET MVC *zobrazení* složky do projektů ASP.NET Core *zobrazení* složky. *Soubor _ViewStart.cshtml* soubor nebyl změněn v aplikaci ASP.NET MVC jádra.

* Vytvoření *zobrazení a sdílených* složky.

* *Volitelné:* kopie *_ViewImports.cshtml* z *FullAspNetCore* projektu MVC *zobrazení* složky do projektů ASP.NET Core  *Zobrazení* složky. Odeberte všechny deklarace oboru názvů v *_ViewImports.cshtml* souboru. *_ViewImports.cshtml* soubor poskytuje obory názvů pro všechny soubory, zobrazení a přináší [značky Pomocníci](xref:mvc/views/tag-helpers/intro). Pomocníci značky se používají v nové rozložení souboru. *_ViewImports.cshtml* souboru je nového pro ASP.NET Core.

* Kopírování *_Layout.cshtml* soubor z původního projektu ASP.NET MVC *zobrazení a sdílených* složky do projektů ASP.NET Core *zobrazení a sdílených* složky.

Otevřete *_Layout.cshtml* souboru a proveďte následující změny (dokončený kód je zobrazené dole):

   * Nahraďte `@Styles.Render("~/Content/css")` s `<link>` elementu, který chcete načíst *bootstrap.css* (viz níže).

   * Odebrat `@Scripts.Render("~/bundles/modernizr")`.

   * Komentář `@Html.Partial("_LoginPartial")` řádku (obklopit řádek s `@*...*@`). Vrátí k němu v budoucnu kurzu.

   * Nahraďte `@Scripts.Render("~/bundles/jquery")` s `<script>` – element (viz níže).

   * Nahraďte `@Scripts.Render("~/bundles/bootstrap")` s `<script>` – element (viz níže)...

Odkaz nahrazení šablon stylů CSS:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

Značky skriptu nahrazení:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

Aktualizovaný *_Layout.cshtml* souboru jsou uvedeny níže:

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

Zobrazte webu v prohlížeči. Nyní by se měly správně načíst s očekávanou styly na místě.

* *Volitelné:* můžete chtít zkuste použít nový soubor rozložení. Pro tento projekt můžete zkopírovat soubor rozložení z *FullAspNetCore* projektu. Nový soubor rozložení používá [značky Pomocníci](xref:mvc/views/tag-helpers/intro) a má další vylepšení.

## <a name="configure-bundling-and-minification"></a>Konfigurace sdružování a minimalizace

Informace o tom, jak nakonfigurovat sdružování a minimalizace najdete v tématu [sdružování a Minifikace](../client-side/bundling-and-minification.md).

## <a name="solving-http-500-errors"></a>Řešení chyb HTTP 500

Existuje mnoho problémů, které můžou způsobit chybová zpráva HTTP 500 které neobsahují žádné informace na zdroj problému. Například pokud *Views/_ViewImports.cshtml* soubor obsahuje obor názvů, který neexistuje v projektu, získáte chyby HTTP 500. Chcete-li získat podrobné chybové zprávy, přidejte následující kód:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

V tématu **pomocí stránky výjimka vývojáře** v [zpracovávat chyby](../fundamentals/error-handling.md) Další informace.

## <a name="additional-resources"></a>Další zdroje

* [Vývoj straně klienta](xref:client-side/index)
* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
