---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Přidání zobrazení (VB) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: c9675eb7776116ecbe910d5515abfe9b4391df22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-view-vb"></a>Přidání zobrazení (VB)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se VB.NET zdrojový kód je k dispozici v tomto tématu. [Stáhnout verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přepnout [C# verze](../cs/adding-a-view.md) tohoto kurzu.


V této části vytvoříme upravit `HelloWorldController` třídu se má použít zobrazení souboru šablony do této aplikace zapouzdření proces generování odpovědi HTML pro klienta.

Začněme pomocí šablony zobrazení s `Index` metoda v `HelloWorldController` třídy. Aktuálně `Index` metoda vrátí řetězec s zprávu, která je pevně zakódovaná v rámci třídy kontroleru. Změna `Index` metoda vrátí `View` objektu, jak je znázorněno v následujícím:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Pojďme teď přidejte šablonu a zobrazení pro naše projekt, který jsme můžete volat pomocí `Index` metoda. Chcete-li to provést, klikněte pravým tlačítkem uvnitř `Index` metoda a klikněte na tlačítko **přidat zobrazení**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**Přidat zobrazení** zobrazí se dialogové okno. Nechte výchozí položky a klikněte na **přidat** tlačítko.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld* složky a *MvcMovie\Views\HelloWorld\Index.vbhtml* soubor se vytvoří. Zobrazí se jim v **Průzkumníku řešení**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Přidat kód HTML v části `<h2>` značky. Upravenou *MvcMovie\Views\HelloWorld\Index.vbhtml* souboru je uveden níže.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Spusťte aplikaci a přejděte do &quot;hello, world&quot; řadiče (`http://localhost:xxxx/HelloWorld`). `Index` Metoda do kontroleru nebyla práci mnohem; jednoduše byl spuštěn příkaz `return View()`, který uvedené, že jsme chtěli použít soubor šablony zobrazení k vykreslení odpovědi klientovi. Protože jsme explicitně nezadali název souboru šablony zobrazení používat, ASP.NET MVC uvedena pomocí *Index.vbhtml* zobrazení souborů v rámci *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec pevně v zobrazení.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Vypadá poměrně funkční. Všimněte si však, že v prohlížeči záhlaví říká &quot;Index&quot; a uvádí velký nadpis na stránce &quot;Moje aplikace MVC.&quot; Umožňuje změnit ty.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

Nejprve změňte text &quot;Moje aplikace MVC.&quot; Tento text se sdílí a se zobrazí na každé stránce. Ve skutečnosti zobrazí se na jenom jednom místě v našem projektu, i když je na každé stránce v naší aplikaci. Přejděte na */zobrazení/sdílené* složky v **Průzkumníku řešení** a otevřete  *\_Layout.vbhtml* souboru. Tento soubor se nazývá ke stránce rozložení a je sdílený &quot;prostředí&quot; , všechny ostatní stránky použít.

Poznámka: `@RenderBody()` řádek kódu v dolní části souboru. `RenderBody` slouží jako zástupný text, kde zobrazit všechny stránky, které vytvoříte, &quot;zabalené&quot; na stránce rozložení. Změna `<h1>` nadpis z **&quot;** Moje aplikace MVC&quot; k &quot;filmová aplikace MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Spusťte aplikaci a poznamenejte si teď uvádí &quot;filmová aplikace MVC&quot;. Klikněte **o** odkaz a že stránka zobrazuje &quot;filmová aplikace MVC&quot;, příliš.

Kompletní  *\_Layout.vbhtml* souboru jsou uvedeny níže:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Nyní změňte název stránky indexu (zobrazení).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Open *MvcMovie\Views\HelloWorld\Index.vbhtml*. Existují dvě místa změnit: nejdřív, zobrazí se text v názvu prohlížeče a pak v hlavičce sekundární ( `<h2>` element). Vytočit je mírně lišit, abyste viděli, které bit kódu změní kterou částí aplikace.

Spusťte aplikaci a přejděte do`http://localhost:xx/HelloWorld`. Všimněte si, že došlo ke změně záhlaví prohlížeče, záhlaví primární a sekundární záhlaví. Je snadné provést big změny ve vaší aplikaci s malým změnám zobrazení. (Pokud nevidíte změny v prohlížeči, může být zobrazil obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči na vynucení odpovědi ze serveru načíst.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Naše trocha &quot;data&quot; (v tomto případě &quot;Hello, World!&quot; zpráva) je pevně zakódovaná, přestože. Naše aplikace MVC má V (zobrazení) a My jsme C (řadiče), ale žádné M (modelu) ještě. Zanedlouho, provedeme procesem vytvoření databáze a načíst data modelu z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z řadiče zobrazení

Před jsme přejděte k databázi a mluvit o modely, ale umožňuje nejprve mluvit o předání informací z řadiče zobrazení. Chceme předat šablonu zobrazení vyžaduje k vykreslení odpovědi HTML pro klienta. Tyto objekty se obvykle vytváří a předaná do zobrazení šablony třídy kontroleru a měly by obsahovat pouze data, která vyžaduje zobrazit šablonu – a žádné další.

Dříve se `HelloWorldController` třídy, `Welcome` trvalo metody akce `name` a `numTimes` parametr a potom výstupní parametr hodnoty do prohlížeče. Spíše než mít řadič pokračovat k vykreslení této odpovědi přímo, místo toho jsme budete přidejme tato data v kontejner pro zobrazení. Kontrolery a zobrazení můžete použít `ViewBag` objekt pro uložení dat. Který se předává do šablony zobrazení automaticky a použije k vykreslení odpovědi HTML pomocí obsah kontejneru objektů a dat jako data. Tímto způsobem se týká jednou z věcí a zobrazení šablony s jiným řadičem – to nám umožňuje udržovat čistou &quot;oddělené oblasti zájmu&quot; v aplikaci.

Alternativně jsme může definovat vlastní třídu, pak vytvoření instance tohoto objektu na vlastní, vyplňte v něm s daty a předejte ji do zobrazení. ViewModel, který se často nazývá protože se jedná vlastní Model pro zobrazení. Pro malé množství dat ale objekt ViewBag vyhovující postup.

Vraťte se do *HelloWorldController.vb* souboru změn `Welcome` metodu řadičem převést zprávu a NumTimes do objekt ViewBag. Objekt ViewBag je dynamický objekt. To znamená, že můžete dát všechno k němu. Objekt ViewBag nemá žádné definované vlastnosti, dokud vložíte něco uvnitř ho.

Kompletní `HelloWorldController.vb` s novou třídu ve stejném souboru.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Naše ViewBag nyní obsahuje data, která se předá zobrazení automaticky. Opakujte případně jsme může mít předaná vlastní objekt takto Pokud jsme líbilo:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nyní potřebujeme `WelcomeView` šablony! Spuštění aplikace, takže kompiluje nový kód. Zavřete prohlížeč, klepněte pravým tlačítkem myši `Welcome` metoda a pak klikněte na tlačítko **přidat zobrazení**.

Tady je co vaše **přidat zobrazení** dialogové okno bude vypadat takto.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Přidejte následující kód pod `<h2>` element v novém <em>úvodní.</em> vbhtml soubor. Jsme budete zkontrolujte smyčku a vyslovte &quot;Hello&quot; tolikrát, kolikrát uživatel zvolí jsme měli!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Spusťte aplikaci a přejděte do `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data se nyní prováděné z adresy URL a automaticky předaný kontroleru. Řadičem zabalí dat do `Model` objekt a předává, které objektu do zobrazení. Zobrazení, než se zobrazí data jako kód HTML pro uživatele.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Také, který byl typ služby &quot;M&quot; modelu, ale není typ databáze. Podívejme se, co jsme jste se naučili a vytvořit databázi filmy.

> [!div class="step-by-step"]
> [Předchozí](adding-a-controller.md)
> [další](adding-a-model.md)
