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
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrace z rozhraní ASP.NET MVC na jádro ASP.NET MVC

Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [ADAM Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), a [Scott Addie](https://scottaddie.com)

Tento článek ukazuje, jak začít s migrací do projektu aplikace ASP.NET MVC [ASP.NET MVC základní](../mvc/overview.md). V procesu se označuje mnoho věcí, které se změnily z rozhraní ASP.NET MVC. Migrace z rozhraní ASP.NET MVC je více proces krok a tento článek se zabývá počáteční nastavení, základní řadiče a zobrazení, statický obsah a závislosti na straně klienta. Další články zahrnovat migraci konfigurace a kód identit v mnoha projekty ASP.NET MVC nalezen.

> [!NOTE]
> Čísla verzí v ukázkách nemusí být aktuální. Musíte aktualizovat projekty, odpovídajícím způsobem.

## <a name="create-the-starter-aspnet-mvc-project"></a>Vytvořit úvodní projektu ASP.NET MVC

K předvedení upgradu, začneme vytvořením aplikace ASP.NET MVC. Vytvořit s názvem *WebApp1* , obor názvů odpovídá projektu ASP.NET Core vytvoříme v dalším kroku.

![Dialogové okno Visual Studio nový projekt](mvc/_static/new-project.png)

![Dialogové okno nové webové aplikace: projektu šablony MVC vybrali panelu šablony ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Volitelné:* změnit název řešení od *WebApp1* k *Mvc5*. Visual Studio zobrazí název nového řešení (*Mvc5*), což usnadňuje říct tento projekt z projektu další.

## <a name="create-the-aspnet-core-project"></a>Vytvoření projektu ASP.NET Core

Vytvořte novou *prázdný* ASP.NET Core webové aplikace se stejným názvem jako předchozí projekt (*WebApp1*), obory názvů v dva projekty shodovat. S o stejný obor názvů usnadňuje zkopírujte kód mezi dva projekty. Budete mít k vytvoření tohoto projektu v jiném adresáři než předchozí projekt, který používá stejný název.

![Dialogové okno Nový projekt](mvc/_static/new_core.png)

![Dialogové okno nové webové aplikace ASP.NET: prázdná šablona projektu vybrané panelu ASP.NET Core šablony](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Volitelné:* vytvoření nové aplikace ASP.NET Core pomocí *webové aplikace* šablona projektu. Název projektu *WebApp1*a vyberte některou možnost ověřování z **jednotlivé uživatelské účty**. Přejmenujte tuto aplikaci a *FullAspNetCore*. Vytvoření tohoto projektu šetří čas v převodu. Můžete si prohlédnout kód generovaný šablony najdete v části konečný výsledek nebo zkopírujte kód do projektu převod. Je také užitečné, pokud zablokuje v kroku převod k porovnání s projektem šablona vytvořena.

## <a name="configure-the-site-to-use-mvc"></a>Konfigurace lokality k použití MVC

* Pokud je cílem .NET Core, metapackage ASP.NET Core se přidá do projektu, názvem `Microsoft.AspNetCore.All` ve výchozím nastavení. Tento balíček obsahuje balíčky jako `Microsoft.AspNetCore.Mvc` a `Microsoft.AspNetCore.StaticFiles`. Pokud cílení na rozhraní .NET Framework, třeba jednotlivě uvedené v souboru *.csproj balíček odkazuje.

`Microsoft.AspNetCore.Mvc` je rozhraní ASP.NET MVC jádra. `Microsoft.AspNetCore.StaticFiles` je obslužná rutina statických souborů. Modul runtime ASP.NET Core je modulární a musí explicitně přihlášení poskytovat statické soubory (viz [statické soubory](xref:fundamentals/static-files)).

* Otevřete *Startup.cs* souboru a změnit kód tak, aby odpovídala následující:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles` Metoda rozšíření přidá obslužné rutiny statických souborů. Jak je uvedeno nahoře, modulem runtime ASP.NET je modulární a musí explicitně přihlášení poskytovat statické soubory. `UseMvc` Přidá metody rozšíření směrování. Další informace najdete v tématu [spuštění aplikace](xref:fundamentals/startup) a [směrování](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Přidání kontroleru a zobrazení

V této části přidáte minimální řadiče a zobrazení, která bude sloužit jako zástupné symboly ASP.NET MVC jsou řadič MVC a zobrazení, že budete migrovat v další části.

* Přidat *řadiče* složky.

* Přidat **třídy Kontroleru** s názvem *HomeController.cs* k *řadiče* složky.

![Přidat novou položku – dialogové okno](mvc/_static/add_mvc_ctl.png)

* Přidat *zobrazení* složky.

* Přidat *zobrazení Domů* složky.

* Přidat **zobrazení syntaxe Razor** s názvem *Index.cshtml* k *zobrazení Domů* složky.

![Přidat novou položku – dialogové okno](mvc/_static/view.png)

Struktura projektu je zobrazena níže:

![Průzkumník řešení zobrazující soubory a složky WebApp1](mvc/_static/project-structure-controller-view.png)

Nahraďte obsah *Views/Home/Index.cshtml* soubor s následující:

```html
<h1>Hello world!</h1>
```

Spusťte aplikaci.

![Webovou aplikaci, otevřete v Microsoft Edge](mvc/_static/hello-world.png)

V tématu [řadiče](xref:mvc/controllers/actions) a [zobrazení](xref:mvc/views/overview) Další informace.

Teď, když máme minimální funkční projekt ASP.NET Core, můžeme začít migrace funkce z projektu ASP.NET MVC. Je potřeba přesunout následující:

* obsah na straně klienta (šablon stylů CSS, písma a skripty)

* kontrolery

* zobrazení

* modely

* Sdružování

* filtry

* Přihlaste se vstup/výstup Identity (to se provádí v dalším kurzu).

## <a name="controllers-and-views"></a>Kontrolery a zobrazení

* Zkopírujte všechny metody z rozhraní ASP.NET MVC `HomeController` do nového `HomeController`. Upozorňujeme, že v architektuře ASP.NET MVC předdefinované šablony řadiče akce metoda návratový typ je [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); v aplikaci ASP.NET MVC jádra, metody akce návratový `IActionResult` místo. `ActionResult` implementuje `IActionResult`, takže není nutné změnit návratový typ vaší metody akce.

* Kopírování *About.cshtml*, *Contact.cshtml*, a *Index.cshtml* Razor zobrazit soubory z projektu ASP.NET MVC do projektu ASP.NET Core.

* Spuštění aplikace ASP.NET Core a testování jednotlivých metod. Jsme jste nemigrovali rozložení souboru nebo styly ještě, takže vykreslené zobrazení obsahovat pouze obsah souborů zobrazení. Nebudete mít rozložení souboru vygenerovaného odkazy `About` a `Contact` zobrazení, takže budete muset je vyvolat z prohlížeče (Nahraďte **4492** číslem portu, na které se používají ve vašem projektu).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Kontaktujte stránky](mvc/_static/contact-page.png)

Poznámka: nedostatek položky stylů a nabídky. To jsme budete opravíme v další části.

## <a name="static-content"></a>Statický obsah

V předchozích verzích rozhraní ASP.NET MVC statický obsah hostitelem byl z kořenového adresáře webového projektu a byl smíšeného s soubory na straně serveru. V ASP.NET Core, statický obsah uložený v *wwwroot* složky. Budete chtít zkopírujte statický obsah z původního aplikaci ASP.NET MVC *wwwroot* složku ve vašem projektu ASP.NET Core. V této ukázkové převod:

* Kopírování *favicon.ico* soubor z původní projekt MVC *wwwroot* složky v projektu ASP.NET Core.

Starý ASP.NET MVC projektu používá [Bootstrap](https://getbootstrap.com/) pro jeho stylů a úložišť, službou Bootstrap nástroje soubory *obsahu* a *skripty* složek. Šablony, která generuje staré projektu ASP.NET MVC, odkazuje na Bootstrap v rozložení souboru (*Views/Shared/_Layout.cshtml*). Může zkopírovat *bootstrap.js* a *bootstrap.css* soubory z rozhraní ASP.NET MVC projektu do *wwwroot* složky v novém projektu. Místo toho přidáme podporu Bootstrap (a další klientské knihovny) pomocí sítím CDN v další části.

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

* Nahraďte `@Scripts.Render("~/bundles/bootstrap")` s `<script>` – element (viz níže).

Nahrazení kód pro zahrnutí Bootstrap CSS:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Nahrazení značky jQuery a Bootstrap JavaScript zahrnutí:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Aktualizovaný *_Layout.cshtml* souboru jsou uvedeny níže:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Zobrazte webu v prohlížeči. Nyní by se měly správně načíst s očekávanou styly na místě.

* *Volitelné:* můžete chtít zkuste použít nový soubor rozložení. Pro tento projekt můžete zkopírovat soubor rozložení z *FullAspNetCore* projektu. Nový soubor rozložení používá [značky Pomocníci](xref:mvc/views/tag-helpers/intro) a má další vylepšení.

## <a name="configure-bundling-and-minification"></a>Konfigurace sdružování a minimalizace

Informace o tom, jak nakonfigurovat sdružování a minimalizace najdete v tématu [sdružování a Minifikace](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Řešení chyb HTTP 500

Existuje mnoho problémů, které můžou způsobit chybová zpráva HTTP 500 které neobsahují žádné informace na zdroj problému. Například pokud *Views/_ViewImports.cshtml* soubor obsahuje obor názvů, který neexistuje v projektu, získáte chyby HTTP 500. Ve výchozím nastavení v aplikacích ASP.NET Core `UseDeveloperExceptionPage` rozšíření je přidán do `IApplicationBuilder` a provést, když je konfigurace *vývoj*. To je podrobně popsaná v následující kód:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core převede neošetřených výjimek ve webové aplikaci do chybové odpovědi HTTP 500. Za normálních okolností podrobnosti o chybě nejsou součástí těchto odpovědí, aby se zabránilo úniku potenciálně citlivých informací o serveru. V tématu **pomocí stránky výjimka vývojáře** v [zpracovávat chyby](../fundamentals/error-handling.md) Další informace.

## <a name="additional-resources"></a>Další zdroje

* [Vývoj klientské strany](xref:client-side/index)
* [Pomocné rutiny značek](xref:mvc/views/tag-helpers/intro)
