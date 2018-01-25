---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: "Přidání zobrazení | Microsoft Docs"
author: Rick-Anderson
description: "Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 60374ddc6754f7e312ad08b420268308a9935bb4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-view"></a>Přidání zobrazení
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části se chystáte upravit `HelloWorldController` třídu se má použít zobrazení soubory šablon do této aplikace zapouzdření proces generování odpovědi HTML pro klienta.

Vytvoříte zobrazení šablony souboru pomocí [zobrazovací modul Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) se poprvé objevila v architektuře ASP.NET MVC 3. Zobrazení syntaxe Razor pro šablony mají *.cshtml* příponu souboru a poskytují elegantní způsob vytvoření HTML výstup pomocí jazyka C#. Syntaxe Razor minimalizuje počet znaků a požadované při zápisu zobrazit šablonu stisknutí kláves a umožňuje rychlé, kapaliny kódování pracovního postupu.

Aktuálně `Index` metoda vrátí řetězec s zprávu, která je pevně zakódovaná ve třídě controller. Změna `Index` metoda vrátí `View` objektu, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index` Metody popsané výše používá šablonu zobrazení pro generování odpovědi HTML do prohlížeče. Metody kontroleru (také označované jako [metody akce](http://rachelappel.com/asp.net-mvc-actionresults-explained)), například `Index` metody popsané výše, obecně vrátit [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (nebo třídy odvozené od [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), není primitivní typy, jako řetězec.

V projektu přidejte šablonu a zobrazení, můžete použít s `Index` metoda. Chcete-li to provést, klikněte pravým tlačítkem uvnitř `Index` metoda a klikněte na tlačítko **přidat zobrazení**.

![](adding-a-view/_static/image1.png)

**Přidat zobrazení** zobrazí se dialogové okno. Ponechte výchozí nastavení způsob jejich a klikněte **přidat** tlačítko:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* složky a *MvcMovie\Views\HelloWorld\Index.cshtml* soubor se vytvoří. Zobrazí se jim v **Průzkumníku řešení**:

![](adding-a-view/_static/image3.png)

Následující ukazuje *Index.cshtml* soubor, který byl vytvořen:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Přidejte následující kód HTML v části `<h2>` značky.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Kompletní *MvcMovie\Views\HelloWorld\Index.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Pokud používáte Visual Studio 2012, v Průzkumníku řešení, klikněte pravým tlačítkem *Index.cshtml* soubor a vyberte **zobrazení v nástroj Page Inspector**.

![PLATFORMY](adding-a-view/_static/image5.png)

[Nástroj Page Inspector kurzu](../../views/using-page-inspector-in-aspnet-mvc.md) obsahuje další informace o tomto nástroji nové.

Alternativně spusťte aplikaci a přejděte do `HelloWorld` řadiče (`http://localhost:xxxx/HelloWorld`). `Index` Metoda do kontroleru nebyla práci mnohem; jednoduše byl spuštěn příkaz `return View()`, které určené, že metoda by měla používat soubor šablony zobrazení k vykreslení odpovědi do prohlížeče. Vzhledem k tomu, že nebyla výslovně zadat název souboru šablony zobrazení pro použití, ASP.NET MVC uvedena pomocí *Index.cshtml* zobrazení souboru v *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec &quot;Hello z našich zobrazit šablonu!&quot; pevně v zobrazení.

![](adding-a-view/_static/image6.png)

Vypadá poměrně funkční. Všimněte si však, že záhlaví prohlížeče ukazuje &quot;indexu My ASP.NET A&quot; a uvádí odkaz velká nahoře na stránce &quot;zde bude vaše logo.&quot; Níže &quot;vaše logo zde.&quot; odkaz o jsou registrace a protokolu odkazy a pod odkazující na domovskou stránku a obraťte se na stránky. Umožňuje změnit některé z nich.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

První, kterou chcete změnit &quot;vaše logo zde.&quot; nadpis v horní části stránky. Tento text je běžné na každou stránku. Jsou ve skutečnosti implementované na jenom jednom místě v projektu, i když se zobrazí na každé stránce v aplikaci. Přejděte na */zobrazení/sdílené* složky v **Průzkumníku řešení** a otevřete  *\_Layout.cshtml* souboru. Tento soubor se nazývá *rozložení stránky* a je sdílený &quot;prostředí&quot; , všechny ostatní stránky použít.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Šablony rozložení umožňují zadat rozložení kontejneru HTML vašeho webu na jednom místě a pak ho použít na více stránkách ve vaší lokalitě. Najít `@RenderBody()` řádku. `RenderBody`je zástupný symbol kde všechny na zobrazení konkrétní stránky, můžete vytvořit zobrazení, &quot;zabalené&quot; na stránce rozložení. Například, pokud vyberete o odkaz *Views\Home\About.cshtml* zobrazení vykresleno uvnitř `RenderBody` metoda.

Změnit záhlaví název webu v šabloně rozložení z &quot;zde bude vaše logo&quot; k &quot;MVC film&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Nahraďte obsah title element následující kód:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Spusťte aplikaci a Všimněte si, že teď uvádí &quot;MVC film &quot;. Klikněte **o** odkaz a v tématu jak na této stránce se zobrazí &quot;MVC film&quot;, příliš. Jsme byli schopni jednou proveďte změny v šabloně rozložení a mít všechny stránky webu odrážet nový název.

![](adding-a-view/_static/image8.png)

Nyní změňte název zobrazení pro Index.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Existují dvě místa změnit: nejdřív, zobrazí se text v názvu prohlížeče a pak v hlavičce sekundární ( `<h2>` element). Budete je provedete mírně lišit, abyste viděli, které bit kódu změní kterou částí aplikace.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

K označení HTML nadpis můžete zobrazit, kód výše sady `Title` vlastnost `ViewBag` objektu (se v *Index.cshtml* zobrazit šablonu). Pokud se podíváte zpátky na zdrojový kód rozložení šablony, můžete si všimnout, že šablona používá tuto hodnotu v `<title>` element jako součást `<head>` části HTML, které jsme dříve upravit. Pomocí této `ViewBag` přístup, můžete snadno předat další parametry mezi zobrazit šablonu a rozložení souboru.

Spusťte aplikaci a přejděte do `http://localhost:xx/HelloWorld`. Všimněte si, že došlo ke změně záhlaví prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud nevidíte změny v prohlížeči, může být zobrazil obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči na vynucení odpovědi ze serveru načíst.) Záhlaví prohlížeče je vytvořena s `ViewBag.Title` nastaví *Index.cshtml* zobrazit šablony a další &quot;-filmová aplikace&quot; přidat v souboru rozložení.

Všimněte si také jak obsah *Index.cshtml* zobrazit šablonu se sloučil s  *\_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Šablony rozložení skutečně usnadňují provést změny, které platí pro všechny stránky v aplikaci.

![](adding-a-view/_static/image9.png)

Naše trocha &quot;data&quot; (v tomto případě &quot;Hello z našich zobrazení šablony!&quot; zpráva) je pevně zakódovaná, přestože. Má aplikace MVC &quot;V&quot; (zobrazení) a máte k dispozici &quot;C&quot; (řadič), ale žádné &quot;M&quot; (modelu) ještě. Zanedlouho, provedeme procesem vytvoření databáze a načíst data modelu z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z řadiče zobrazení

Před jsme přejděte k databázi a mluvit o modely, ale umožňuje nejprve mluvit o předání informací z řadiče zobrazení. Třídy kontroleru je vyvolán v odpovědi na požadavek příchozí adresy URL. Třída kontroleru je, kde můžete psát kód, který zpracovává příchozí prohlížeče požadavky, načte data z databáze a nakonec rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony pak lze z řadiče pro vygenerování a formátování odpovědi HTML do prohlížeče.

Řadiče jsou zajišťuje ať data nebo objekty jsou potřeba v pořadí pro šablonu zobrazení k vykreslení odpovědi do prohlížeče. Osvědčeným postupem: **by měla zobrazit šablonu nikdy provádět obchodní logiku nebo pracovat přímo s databází**. Místo toho by měla zobrazit šablonu fungovat jenom s data, která je k němu poskytované řadičem. Zachování to &quot;oddělené oblasti zájmu&quot; pomáhá udržovat kódu čistá, možností intenzivního testování a více udržovatelný.

V současné době `Welcome` metodu akce v `HelloWorldController` třídy trvá `name` a `numTimes` parametr a potom výstupy hodnoty přímo do prohlížeče. Místo mít řadič vykreslení této odpovědi jako řetězec, umožňuje změnit řadič místo toho použít šablonu zobrazení. Zobrazit šablonu způsobí vygenerování dynamických odpovědí, která znamená, že potřebujete předat příslušné bits dat z řadiče zobrazení za účelem vygenerování odpovědi. To provedete tak, že řadiče put dynamických dat (parametry), která vyžaduje zobrazení šablony `ViewBag` objekt, který šablona zobrazení můžete poté přistoupit.

Vraťte se do *HelloWorldController.cs* soubor a změňte `Welcome` metody přidat `Message` a `NumTimes` hodnotu `ViewBag` objektu. `ViewBag`je to dynamický objekt, což znamená, že všechno můžete vložit do ní; `ViewBag` objekt nemá žádné definované vlastnosti, dokud vložíte něco uvnitř ho. [Systému vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapuje pojmenované parametry (`name` a `numTimes`) z řetězce dotazu v panelu Adresa parametry ve své metodě. Kompletní *HelloWorldController.cs* soubor vypadá takto:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nyní `ViewBag` objekt obsahuje data, která bude předána do zobrazení automaticky.

Dále je nutné zadat šablonu úvodní zobrazení! V **sestavení** nabídce vyberte možnost **sestavení MvcMovie** a ujistěte se, kompilace projektu.

Klikněte pravým tlačítkem uvnitř `Welcome` metoda a klikněte na tlačítko **přidat zobrazení**.

![](adding-a-view/_static/image10.png)

Tady je co **přidat zobrazení** dialogové okno bude vypadat takto:

![](adding-a-view/_static/image11.png)

Klikněte na tlačítko **přidat**a poté přidejte následující kód pod `<h2>` element v novém *Welcome.cshtml* souboru. Vytvoříte smyčku, která uvádí, že &quot;Hello&quot; tolikrát, kolikrát uživatel zvolí by měl. Kompletní *Welcome.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Spusťte aplikaci a přejděte na následující adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data jsou převzaty z adresy URL a předaný pomocí řadiče [vazač modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Balíčky data do kontroleru `ViewBag` objekt a předává, které objektu do zobrazení. Zobrazení pak zobrazí data jako kód HTML pro uživatele.

![](adding-a-view/_static/image12.png)

V ukázce výše jsme použili `ViewBag` objekt, který chcete předat data z řadiče zobrazení. Tomu v tomto kurzu budeme používat model zobrazení k předávání dat z řadiče zobrazení. Zobrazení modelu přístupu k předávání dat je obvykle mnohem upřednostňované nad přístup kontejner objektů a dat zobrazení. Najdete v položkách blogu [dynamická V silného typu zobrazení](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Další informace.

Také, který byl typ služby &quot;M&quot; modelu, ale není typ databáze. Podívejme se, co jsme jste se naučili a vytvořit databázi filmy.

>[!div class="step-by-step"]
[Předchozí](adding-a-controller.md)
[další](adding-a-model.md)
