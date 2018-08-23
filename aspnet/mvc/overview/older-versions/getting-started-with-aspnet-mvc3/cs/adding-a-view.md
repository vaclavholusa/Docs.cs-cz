---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Přidání zobrazení (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 4254bfa1098ed2d2d850c00a44e94458ad2e7a54
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752844"
---
<a name="adding-a-view-c"></a>Přidání zobrazení (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu. [Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


V této části se chystáte změnit `HelloWorldController` třídu použít zobrazení soubory šablon do čistě zapouzdření proces generování odpovědi HTML do klienta.

Vytvoříte soubor šablony zobrazení pomocí nového [zobrazovací modul Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) zavedené v architektuře ASP.NET MVC 3. Šablony Razor na základě zobrazení mají *.cshtml* příponu souboru a poskytují elegantní způsob, jak vytvořit HTML výstup pomocí jazyka C#. Razor minimalizuje počet znaků a vyžaduje při psaní zobrazit šablonu stisknutí kláves a umožňuje rychlé, dynamika kódování pracovního postupu.

Začněte tím, že pomocí zobrazení šablony s `Index` metoda ve `HelloWorldController` třídy. Aktuálně `Index` metoda vrátí řetězec a zobrazí se zpráva, která je pevně zakódovaný ve třídě controller. Změnit `Index` metodu pro návrat `View` objektu, jak je znázorněno v následujícím:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Tento kód používá šablonu zobrazení k vygenerování odpověď ve formátu HTML v prohlížeči. V projektu, přidat zobrazení šablony, který vám pomůže s `Index` metody. Chcete-li to provést, klepněte pravým tlačítkem myši `Index` metoda a klikněte na tlačítko **přidat zobrazení**.

![](adding-a-view/_static/image1.png)

**Přidat zobrazení** zobrazí se dialogové okno. Ponechte výchozí nastavení, jako jsou a klikněte na tlačítko **přidat** tlačítka:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* složky a *MvcMovie\Views\HelloWorld\Index.cshtml* se vytvoří soubor. Zobrazí se jim v **Průzkumníka řešení**:

![](adding-a-view/_static/image3.png)

Zobrazí se následující *Index.cshtml* soubor, který byl vytvořen:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Přidat kód HTML v části `<h2>` značky. Upravené *MvcMovie\Views\HelloWorld\Index.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Spusťte aplikaci a přejděte `HelloWorld` kontroler (`http://localhost:xxxx/HelloWorld`). `Index` Metoda ve vašem řadiči nedělalo množství práce, jednoduše spustili příkaz `return View()`, která zadané, že metoda by měla používat soubor šablony zobrazení k vykreslení odezva do prohlížeče. Protože jste nezadali explicitní název souboru šablony zobrazení pro použití, ASP.NET MVC na výchozí použití *Index.cshtml* zobrazení souboru v *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec pevně zakódované v zobrazení.

![](adding-a-view/_static/image6.png)

Vypadá hodně Dobrá. Všimněte si však, že prohlížeče záhlaví říká "Index", velké objemy nadpis na stránce říká "Moje aplikace MVC." Změňme ty.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

Nejprve budete chtít změnit název "Moje aplikace MVC" v horní části stránky. Tento text je společné pro všechny stránky. Ve skutečnosti je implementován na jenom jednom místě v projektu, i když se zobrazí na každé stránce v aplikaci. Přejděte */zobrazení/Shared* složky v **Průzkumníku řešení** a otevřete  *\_Layout.cshtml* souboru. Tento soubor se nazývá *stránku rozložení* a je sdílený "prostředí", všechny ostatní stránky použít.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Šablony rozložení umožňují zadat kontejner rozložení HTML vašeho webu na jednom místě a použijte ji na několika stránkách ve vaší lokalitě. Poznámka: `@RenderBody()` řádku v dolní části souboru. `RenderBody` zástupným symbolem, kde všechny specifické pro zobrazení stránky, které můžete vytvořit zobrazení, "zabalené" na stránce rozložení. Změna názvu záhlaví v šabloně rozložení z "My MVC Application" do "Aplikace MVC Movie".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Spusťte aplikaci a Všimněte si, že nyní říká "Aplikace MVC Movie". Klikněte na tlačítko **o** odkaz a uvidíte, jak této stránce se zobrazí "Aplikace MVC Movie", příliš. Jsme byli schopni jednou provést změnu v šabloně rozložení a mít všechny stránky na webu odrážely nový název.

![](adding-a-view/_static/image9.png)

Kompletní  *\_Layout.cshtml* souboru je uveden níže:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Teď Změníme název indexovou stránku (Zobrazit).

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Existují dvě místa, kde provést změnu: první, text, který se zobrazí v záhlaví prohlížeče a v hlavičce sekundární ( `<h2>` element). Budete je provedete trochu jinak, uvidíte, jaké verze kódu se změní kterou část aplikace.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

K označení názvu HTML zobrazení kódu nad sady `Title` vlastnost `ViewBag` objektu (což je v *Index.cshtml* zobrazit šablonu). Pokud podíváte zpátky na zdrojový kód šablony rozložení, můžete si všimnout, že šablona používá tuto hodnotu v `<title>` element jako součást `<head>` část kódu HTML. Tento přístup můžete snadno předat další parametry mezi zobrazit šablonu a soubor rozložení.

Spusťte aplikaci a přejděte do `http://localhost:xx/HelloWorld`. Všimněte si, že došlo ke změně názvu prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud se nezobrazí změny v prohlížeči, pravděpodobně jste zobrazili obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči k vynucení odpověď ze serveru, který se má načíst.)

Všimněte si také způsob, jak v obsahu *Index.cshtml* zobrazit šablonu byl sloučen s  *\_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Rozložení šablony usnadňují skutečně provést změny, které platí pro všechny stránky v aplikaci.

![](adding-a-view/_static/image10.png)

Naše něco "data" (v tomto případě "Hello z našich zobrazit šablonu!" zpráva) je pevně zakódované, i když. Aplikace MVC má "V" (Zobrazit) a máte "C" (controller), ale žádné "M" (modelu) ještě. Po chvíli, projdeme postup vytvoření databáze a načíst datový model z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z Kontroleru zobrazení

Předtím, než jsme přejděte k databázi a mluvit o modely, ale nejprve se seznámíme předávání informací z kontroleru zobrazení. Kontroler třídy jsou vyvolány v reakci na příchozí adrese URL žádosti. Třída kontroleru je místo, kde píšete kód, který zpracovává příchozí parametry, načte data z databáze a nakonec rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony pak lze z kontroleru pro generování a formátovat odpověď ve formátu HTML v prohlížeči.

Kontrolery odpovídají za poskytování libovolné dat nebo objekty jsou nutné v pořadí pro šablonu zobrazení k vykreslení odezva do prohlížeče. Zobrazit šablonu by nikdy provádění obchodní logiky nebo pracovat přímo s databází. Místo toho by měla fungovat jenom s daty, která je poskytována kontroleru. Zachování tohoto "oddělené oblasti zájmu" pomáhá chránit váš kód čisté snadněji spravovatelné.

V současné době `Welcome` metodu akce v `HelloWorldController` třídy přijímá `name` a `numTimes` parametr a potom výstupy hodnoty přímo do prohlížeče. Spíše než mít řadič vykreslení této odpovědi jako řetězec, Změníme kontroler místo toho použít šablonu zobrazení. Zobrazit šablonu vygeneruje dynamické odpovědi, což znamená, že je potřeba předat odpovídající částí dat z kontroleru zobrazení k vygenerování odpovědi. Můžete to provést tak, že kontroler umístit dynamických dat, která vyžaduje zobrazení šablony `ViewBag` objekt, který se pak můžou zobrazit šablonu.

Vraťte se na *HelloWorldController.cs* soubor a změňte `Welcome` metoda pro přidání `Message` a `NumTimes` hodnota, která se `ViewBag` objektu. `ViewBag` je to dynamický objekt, což znamená, že můžete vložit cokoliv, co chcete `ViewBag` objekt nemá žádné definované vlastnosti, dokud je něco v ji vložíte. Kompletní *HelloWorldController.cs* souboru vypadá takto:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Nyní `ViewBag` objekt obsahuje data, která bude předána do zobrazení automaticky.

Dále je třeba úvodní zobrazit šablonu! V **ladění** nabídce vyberte možnost **sestavení MvcMovie** k Ujistěte se, že je projekt kompilován.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Klikněte pravým tlačítkem na uvnitř `Welcome` metoda a klikněte na tlačítko **přidat zobrazení**. Co **přidat zobrazení** dialogové okno bude vypadat takto:

![](adding-a-view/_static/image13.png)

Klikněte na tlačítko **přidat**a poté přidejte následující kód `<h2>` element na novém *Welcome.cshtml* souboru. Vytvoříte smyčku, která říká "Hello" tolikrát, kolikrát uživatel říká, že by měl. Kompletní *Welcome.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Spusťte aplikaci a přejděte na následující adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data je teď na základě adresy URL a automaticky předán kontroleru. Balíčky data do kontroleru `ViewBag` objektu a, které se objekt předá do zobrazení. Zobrazení pak zobrazí data jako kód HTML pro uživatele.

![](adding-a-view/_static/image14.png)

Dobře, to bylo druh "M" pro model, ale není typ databáze. Pojďme se na to co jsme se naučili a vytvořit databázi videa.

> [!div class="step-by-step"]
> [Předchozí](adding-a-controller.md)
> [další](adding-a-model.md)
