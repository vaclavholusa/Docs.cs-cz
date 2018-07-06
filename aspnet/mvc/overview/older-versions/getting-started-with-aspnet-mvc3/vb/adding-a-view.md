---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Přidání zobrazení (VB) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 140dc8a7302c52112c2d8873325b2567e3ba66a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829976"
---
<a name="adding-a-view-vb"></a>Přidání zobrazení (VB)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem VB.NET je k dispozici v tomto tématu. [Stáhněte si verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přejděte [verze jazyka C#](../cs/adding-a-view.md) tohoto kurzu.


V této části chceme změnit `HelloWorldController` třídu použít soubor zobrazení šablony k čistě zapouzdření proces generování odpovědi HTML do klienta.

Začněme tím, že pomocí zobrazení šablony s `Index` metodu `HelloWorldController` třídy. Aktuálně `Index` metoda vrátí řetězec a zobrazí se zpráva, která je pevně zakódovaný v rámci třídy kontroleru. Změnit `Index` metodu pro návrat `View` objektu, jak je znázorněno v následujícím:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Teď přidáme zobrazit šablonu pro náš projekt, který jsme lze vyvolat pomocí `Index` metody. Chcete-li to provést, klepněte pravým tlačítkem myši `Index` metoda a klikněte na tlačítko **přidat zobrazení**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

**Přidat zobrazení** zobrazí se dialogové okno. Ponechte výchozí položky a klikněte na tlačítko **přidat** tlačítko.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

*MvcMovie\Views\HelloWorld* složky a *MvcMovie\Views\HelloWorld\Index.vbhtml* se vytvoří soubor. Zobrazí se jim v **Průzkumníka řešení**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Přidat kód HTML v části `<h2>` značky. Upravené *MvcMovie\Views\HelloWorld\Index.vbhtml* souboru je uveden níže.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Spusťte aplikaci a přejděte &quot;hello world&quot; kontroler (`http://localhost:xxxx/HelloWorld`). `Index` Metoda ve vašem řadiči nedělalo množství práce, jednoduše spustili příkaz `return View()`, které označeny jsme chtěli použít soubor šablony zobrazení k vykreslení odpovědi klientovi. Protože jsme explicitně neurčil název souboru šablony zobrazení pro použití, ASP.NET MVC na výchozí použití *Index.vbhtml* zobrazení souboru v rámci *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec pevně zakódované v zobrazení.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Vypadá hodně Dobrá. Všimněte si však, že v prohlížeči na záhlaví říká &quot;Index&quot; a uvádí, že velké objemy nadpis na stránce &quot;Moje aplikace MVC.&quot; Změňme ty.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

Nejprve můžeme změnit text &quot;Moje aplikace MVC.&quot; Tento text se sdílí a se zobrazí na každé stránce. Ve skutečnosti zobrazí se na jenom jednom místě v našem projektu, i když je na každé stránce v naší aplikaci. Přejděte */zobrazení/Shared* složky v **Průzkumníku řešení** a otevřete  *\_Layout.vbhtml* souboru. Tento soubor se nazývá rozložení stránky a je sdílený &quot;prostředí&quot; , všechny ostatní stránky použít.

Poznámka: `@RenderBody()` řádek kódu v dolní části souboru. `RenderBody` zástupným symbolem, kde se zobrazuje na stránkách, které vytvoříte, &quot;zabalené&quot; stránce rozložení. Změnit `<h1>` nadpis z **&quot;** Moje aplikace MVC&quot; k &quot;filmová aplikace MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Spusťte aplikaci a poznamenejte si ho teď říká &quot;filmová aplikace MVC&quot;. Klikněte na tlačítko **o** odkaz a že stránka zobrazuje &quot;filmová aplikace MVC&quot;také.

Kompletní  *\_Layout.vbhtml* souboru je uveden níže:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teď Změníme název indexovou stránku (Zobrazit).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Open *MvcMovie\Views\HelloWorld\Index.vbhtml*. Existují dvě místa, kde provést změnu: první, text, který se zobrazí v záhlaví prohlížeče a v hlavičce sekundární ( `<h2>` element). Uděláme je trochu jinak, uvidíte, jaké verze kódu se změní kterou část aplikace.

Spusťte aplikaci a přejděte do`http://localhost:xx/HelloWorld`. Všimněte si, že došlo ke změně názvu prohlížeče, záhlaví primární a sekundární záhlaví. Je snadné vytvořit velké změny ve vaší aplikace pomocí malé změny do zobrazení. (Pokud se nezobrazí změny v prohlížeči, pravděpodobně jste zobrazili obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči k vynucení odpověď ze serveru, který se má načíst.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Naše nepatrné &quot;data&quot; (v tomto případě &quot;Hello World!&quot; zprávu) je pevně zakódované, i když. Naše aplikace MVC má V (zobrazení) a My máme C (kontrolery), ale žádné M (modelu) ještě. Po chvíli, projdeme postup vytvoření databáze a načíst datový model z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z Kontroleru zobrazení

Předtím, než jsme přejděte k databázi a mluvit o modely, ale nejprve se seznámíme předávání informací z Kontroleru zobrazení. Chceme předat zobrazení šablony vyžaduje k vykreslení odpověď ve formátu HTML klienta. Tyto objekty jsou obvykle vytvořen a předávány třídu kontroleru zobrazení šablony a měly by obsahovat pouze data, která vyžaduje zobrazení šablony – a žádných dalších.

Dříve se `HelloWorldController` třídy, `Welcome` metody akce `name` a `numTimes` parametr a potom výstupní hodnoty parametrů v prohlížeči. Raději než mít řadič se nadále vykreslují tuto odpověď přímo, můžeme místo toho jsme jako umístění vyberu tato data kontejner pro zobrazení. Můžete použít kontrolerů a zobrazení `ViewBag` objekt pro uložení data. Který se předává do zobrazení šablony automaticky a použije k vykreslení odpovědi HTML pomocí obsahu kontejner objektů a dat jako data. Díky tomu se týká jednou z věcí a zobrazení šablony s jiným řadičem – umožňuje nám udržovat čisté &quot;oddělení oblastí zájmu&quot; v rámci aplikace.

Také jsme může definovat vlastní třídu, pak vytvořit instanci tohoto objektu na naše vlastní, vyplní jej s daty a předána do zobrazení. Který se často nazývá ViewModel, protože se jedná vlastní Model pro zobrazení. Pro malá množství dat ale objekt ViewBag spolupráce.

Zpět *HelloWorldController.vb* změna souboru `Welcome` metodu řadiče převést zprávu a NumTimes do objekt ViewBag. Objekt ViewBag je to dynamický objekt. To znamená, že můžete umístit cokoliv, co chcete k němu. Objekt ViewBag nemá žádné definované vlastnosti, dokud je něco v ji vložíte.

Kompletní `HelloWorldController.vb` s novou třídu do stejného souboru.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Náš objekt ViewBag nyní obsahuje data, která se předá zobrazení automaticky. Opakujte případně jsme může prošly v naší vlastní objekt takto Pokud zalíbila se nám:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Teď potřebujeme `WelcomeView` šablony! Spusťte aplikaci, aby nový kód je zkompilován. Zavřete prohlížeč, klepněte pravým tlačítkem myši `Welcome` metody a pak klikněte na tlačítko **přidat zobrazení**.

Tady je co vaše **přidat zobrazení** dialogové okno bude vypadat takto.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Přidejte následující kód `<h2>` element na novém <em>úvodní.</em> vbhtml souboru. Vytvoříme Ujistěte se, smyčky a Řekněme, že &quot;Hello&quot; tolikrát, kolikrát uživatel zvolí, měli bychom!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Spusťte aplikaci a přejděte na `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data je teď na základě adresy URL a automaticky předán kontroleru. Kontroler zabalí dat do `Model` objektu a, které se objekt předá do zobrazení. Zobrazení, než se uživateli zobrazí data ve formátu HTML.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Kontejneru, který byl typ z &quot;M&quot; pro model, ale není typ databáze. Pojďme se na to co jsme se naučili a vytvořit databázi videa.

> [!div class="step-by-step"]
> [Předchozí](adding-a-controller.md)
> [další](adding-a-model.md)
