---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Přidání zobrazení (C#) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873167"
---
<a name="adding-a-view-c"></a>Přidání zobrazení (C#)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


V této části se chystáte upravit `HelloWorldController` třídu se má použít zobrazení soubory šablon do této aplikace zapouzdření proces generování odpovědi HTML pro klienta.

Vytvoříte pomocí nového souboru šablony zobrazení [zobrazovací modul Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) se poprvé objevila v architektuře ASP.NET MVC 3. Zobrazení syntaxe Razor pro šablony mají *.cshtml* příponu souboru a poskytují elegantní způsob vytvoření HTML výstup pomocí jazyka C#. Syntaxe Razor minimalizuje počet znaků a požadované při zápisu zobrazit šablonu stisknutí kláves a umožňuje rychlé, kapaliny kódování pracovního postupu.

Spustit pomocí šablony zobrazení s `Index` metoda v `HelloWorldController` třídy. Aktuálně `Index` metoda vrátí řetězec s zprávu, která je pevně zakódovaná ve třídě controller. Změna `Index` metoda vrátí `View` objektu, jak je znázorněno v následujícím:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Tento kód používá šablonu zobrazení pro generování odpovědi HTML do prohlížeče. V projektu přidejte šablonu a zobrazení, můžete použít s `Index` metoda. Chcete-li to provést, klikněte pravým tlačítkem uvnitř `Index` metoda a klikněte na tlačítko **přidat zobrazení**.

![](adding-a-view/_static/image1.png)

**Přidat zobrazení** zobrazí se dialogové okno. Ponechte výchozí nastavení způsob jejich a klikněte **přidat** tlačítko:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* složky a *MvcMovie\Views\HelloWorld\Index.cshtml* soubor se vytvoří. Zobrazí se jim v **Průzkumníku řešení**:

![](adding-a-view/_static/image3.png)

Následující ukazuje *Index.cshtml* soubor, který byl vytvořen:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Přidat kód HTML v části `<h2>` značky. Upravenou *MvcMovie\Views\HelloWorld\Index.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Spusťte aplikaci a přejděte do `HelloWorld` řadiče (`http://localhost:xxxx/HelloWorld`). `Index` Metoda do kontroleru nebyla práci mnohem; jednoduše byl spuštěn příkaz `return View()`, které určené, že metoda by měla používat soubor šablony zobrazení k vykreslení odpovědi do prohlížeče. Vzhledem k tomu, že nebyla výslovně zadat název souboru šablony zobrazení pro použití, ASP.NET MVC uvedena pomocí *Index.cshtml* zobrazení souboru v *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec pevně v zobrazení.

![](adding-a-view/_static/image6.png)

Vypadá poměrně funkční. Všimněte si však, že v prohlížeči záhlaví říká "Index" a big nadpis na stránce říká "Moje aplikace MVC." Umožňuje změnit ty.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

První kterou chcete změnit "Moje aplikace MVC" nadpis v horní části stránky. Tento text je běžné na každou stránku. Ve skutečnosti jsou implementované na jenom jednom místě v projektu, i když se zobrazí na každé stránce v aplikaci. Přejděte na */zobrazení/sdílené* složky v **Průzkumníku řešení** a otevřete  *\_Layout.cshtml* souboru. Tento soubor se nazývá *rozložení stránky* a je sdílený "prostředí", všechny ostatní stránky použít.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Šablony rozložení umožňují zadat rozložení kontejneru HTML vašeho webu na jednom místě a pak ho použít na více stránkách ve vaší lokalitě. Poznámka: `@RenderBody()` řádku v dolní části souboru. `RenderBody` slouží jako zástupný text, kde všechny zobrazení konkrétní stránky, které vytvoříte objeví, "zabalené" na stránce rozložení. Změňte název záhlaví v šabloně rozložení z "Aplikace My MVC" na "Filmová aplikace MVC".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Spusťte aplikaci a Všimněte si, že nyní říká "Filmová aplikace MVC". Klikněte **o** odkaz a v tématu jak této stránce zobrazuje "Filmová aplikace MVC", příliš. Jsme byli schopni jednou proveďte změny v šabloně rozložení a mít všechny stránky webu odrážet nový název.

![](adding-a-view/_static/image9.png)

Kompletní  *\_Layout.cshtml* souboru jsou uvedeny níže:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Nyní změňte název stránky indexu (zobrazení).

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Existují dvě místa změnit: nejdřív, zobrazí se text v názvu prohlížeče a pak v hlavičce sekundární ( `<h2>` element). Budete je provedete mírně lišit, abyste viděli, které bit kódu změní kterou částí aplikace.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

K označení HTML nadpis můžete zobrazit, kód výše sady `Title` vlastnost `ViewBag` objektu (se v *Index.cshtml* zobrazit šablonu). Pokud se podíváte zpátky na zdrojový kód rozložení šablony, můžete si všimnout, že šablona používá tuto hodnotu v `<title>` element jako součást `<head>` části kódu HTML. Použití tohoto přístupu, můžete snadno předat další parametry mezi zobrazit šablonu a rozložení souboru.

Spusťte aplikaci a přejděte do `http://localhost:xx/HelloWorld`. Všimněte si, že došlo ke změně záhlaví prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud nevidíte změny v prohlížeči, může být zobrazil obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči na vynucení odpovědi ze serveru načíst.)

Všimněte si také jak obsah *Index.cshtml* zobrazit šablonu se sloučil s  *\_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Šablony rozložení skutečně usnadňují provést změny, které platí pro všechny stránky v aplikaci.

![](adding-a-view/_static/image10.png)

Naše trocha "data" (v tomto případě "Hello z našich zobrazit šablonu!" zpráva) je pevně zakódovaná, přestože. Aplikace MVC má "V" (zobrazení) a máte hodně "C" (řadič), ale žádné "M" (modelu) ještě. Zanedlouho, provedeme procesem vytvoření databáze a načíst data modelu z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z řadiče zobrazení

Před jsme přejděte k databázi a mluvit o modely, ale umožňuje nejprve mluvit o předání informací z řadiče zobrazení. Třídy kontroleru je vyvolán v odpovědi na požadavek příchozí adresy URL. Třída kontroleru je, kde můžete napsat kód, který zpracovává příchozí parametry, načte data z databáze a nakonec rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony pak lze z řadiče pro vygenerování a formátování odpovědi HTML do prohlížeče.

Řadiče jsou zajišťuje ať data nebo objekty jsou potřeba v pořadí pro šablonu zobrazení k vykreslení odpovědi do prohlížeče. Zobrazit šablonu by nikdy provádět obchodní logiku nebo pracovat přímo s databází. Místo toho by měla fungovat jenom s daty, která poskytuje k němu kontroleru. Zachování tento "oddělené oblasti zájmu" pomáhá chránit váš kód čistou více udržovatelný.

V současné době `Welcome` metodu akce v `HelloWorldController` třídy trvá `name` a `numTimes` parametr a potom výstupy hodnoty přímo do prohlížeče. Místo mít řadič vykreslení této odpovědi jako řetězec, umožňuje změnit řadič místo toho použít šablonu zobrazení. Zobrazit šablonu způsobí vygenerování dynamických odpovědí, která znamená, že potřebujete předat příslušné bits dat z řadiče zobrazení za účelem vygenerování odpovědi. To provedete tak, že řadiče put dynamických dat, která vyžaduje zobrazení šablony `ViewBag` objekt, který šablona zobrazení můžete poté přistoupit.

Vraťte se do *HelloWorldController.cs* soubor a změňte `Welcome` metody přidat `Message` a `NumTimes` hodnotu `ViewBag` objektu. `ViewBag` je to dynamický objekt, což znamená, že všechno můžete vložit do ní; `ViewBag` objekt nemá žádné definované vlastnosti, dokud vložíte něco uvnitř ho. Kompletní *HelloWorldController.cs* soubor vypadá takto:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Nyní `ViewBag` objekt obsahuje data, která bude předána do zobrazení automaticky.

Dále je nutné zadat šablonu úvodní zobrazení! V **ladění** nabídce vyberte možnost **sestavení MvcMovie** a ujistěte se, kompilace projektu.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Klikněte pravým tlačítkem uvnitř `Welcome` metoda a klikněte na tlačítko **přidat zobrazení**. Tady je co **přidat zobrazení** dialogové okno bude vypadat takto:

![](adding-a-view/_static/image13.png)

Klikněte na tlačítko **přidat**a poté přidejte následující kód pod `<h2>` element v novém *Welcome.cshtml* souboru. Vytvoříte smyčku, s upozorněním, kolikrát uživatel informacemi o tom, že by měl "text Hello". Kompletní *Welcome.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Spusťte aplikaci a přejděte na následující adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data se nyní prováděné z adresy URL a automaticky předaný kontroleru. Balíčky data do kontroleru `ViewBag` objekt a předává, které objektu do zobrazení. Zobrazení pak zobrazí data jako kód HTML pro uživatele.

![](adding-a-view/_static/image14.png)

Dobře, který byl druh "M" pro model, ale není typ databáze. Podívejme se, co jsme jste se naučili a vytvořit databázi filmy.

> [!div class="step-by-step"]
> [Předchozí](adding-a-controller.md)
> [další](adding-a-model.md)
