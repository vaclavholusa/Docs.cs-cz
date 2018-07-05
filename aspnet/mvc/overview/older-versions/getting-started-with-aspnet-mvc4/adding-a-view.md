---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Přidání zobrazení | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 0974cd2e06ed86c736214944a29a5a1eab8af50b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391506"
---
<a name="adding-a-view"></a>Přidání zobrazení
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části se chystáte změnit `HelloWorldController` třídu použít zobrazení soubory šablon do čistě zapouzdření proces generování odpovědi HTML do klienta.

Vytvoříte soubor šablony zobrazení pomocí [zobrazovací modul Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) zavedené v architektuře ASP.NET MVC 3. Šablony Razor na základě zobrazení mají *.cshtml* příponu souboru a poskytují elegantní způsob, jak vytvořit HTML výstup pomocí jazyka C#. Razor minimalizuje počet znaků a vyžaduje při psaní zobrazit šablonu stisknutí kláves a umožňuje rychlé, dynamika kódování pracovního postupu.

Aktuálně `Index` metoda vrátí řetězec a zobrazí se zpráva, která je pevně zakódovaný ve třídě controller. Změnit `Index` metodu pro návrat `View` objektu, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

`Index` Výše uvedené metody používá šablonu zobrazení k vygenerování odpověď ve formátu HTML v prohlížeči. Metody kontroleru (označované také jako [metody akce](http://rachelappel.com/asp.net-mvc-actionresults-explained)), například `Index` metody popsané výše, obvykle vracet [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (nebo třída odvozená z [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), není primitivní typy, jako je řetězec.

V projektu, přidat zobrazení šablony, který vám pomůže s `Index` metody. Chcete-li to provést, klepněte pravým tlačítkem myši `Index` metoda a klikněte na tlačítko **přidat zobrazení**.

![](adding-a-view/_static/image1.png)

**Přidat zobrazení** zobrazí se dialogové okno. Ponechte výchozí nastavení, jako jsou a klikněte na tlačítko **přidat** tlačítka:

![](adding-a-view/_static/image2.png)

*MvcMovie\Views\HelloWorld* složky a *MvcMovie\Views\HelloWorld\Index.cshtml* se vytvoří soubor. Zobrazí se jim v **Průzkumníka řešení**:

![](adding-a-view/_static/image3.png)

Zobrazí se následující *Index.cshtml* soubor, který byl vytvořen:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Přidejte následující kód HTML v části `<h2>` značky.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Kompletní *MvcMovie\Views\HelloWorld\Index.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Pokud používáte sadu Visual Studio 2012, v Průzkumníku řešení, klikněte pravým tlačítkem myši *Index.cshtml* a vyberte možnost **zobrazení v nástroje Page Inspector**.

![PI](adding-a-view/_static/image5.png)

[Nástroj Page Inspector kurzu](../../views/using-page-inspector-in-aspnet-mvc.md) obsahuje další informace o tento nový nástroj.

Alternativně spusťte aplikaci a přejděte `HelloWorld` kontroler (`http://localhost:xxxx/HelloWorld`). `Index` Metoda ve vašem řadiči nedělalo množství práce, jednoduše spustili příkaz `return View()`, která zadané, že metoda by měla používat soubor šablony zobrazení k vykreslení odezva do prohlížeče. Protože jste nezadali explicitní název souboru šablony zobrazení pro použití, ASP.NET MVC na výchozí použití *Index.cshtml* zobrazení souboru v *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec &quot;Hello z našich zobrazit šablonu!&quot; pevně zakódované v zobrazení.

![](adding-a-view/_static/image6.png)

Vypadá hodně Dobrá. Všimněte si však, že v prohlížeči na záhlaví okna zobrazuje &quot;indexu Moje ASP.NET A&quot; a uvádí, že velké objemy odkaz nahoře na stránce &quot;vaše logo.&quot; Níže &quot;vaše logo sem.&quot; odkazu jsou informace o registraci a protokolu v odkazech a níže, který odkazuje na domovskou stránku a obraťte se na stránky. Změňme některé z nich.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

Nejprve, kterou chcete změnit &quot;vaše logo sem.&quot; nadpis v horní části stránky. Tento text je společné pro všechny stránky. Ve skutečnosti implementaci na jenom jednom místě v projektu, i když se zobrazí na každé stránce v aplikaci. Přejděte */zobrazení/Shared* složky v **Průzkumníku řešení** a otevřete  *\_Layout.cshtml* souboru. Tento soubor se nazývá *stránku rozložení* a je sdílený &quot;prostředí&quot; , všechny ostatní stránky použít.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Šablony rozložení umožňují zadat kontejner rozložení HTML vašeho webu na jednom místě a použijte ji na několika stránkách ve vaší lokalitě. Najít `@RenderBody()` řádku. `RenderBody` je zástupný symbol, kde všechny v zobrazení konkrétní stránky, můžete vytvořit, zobrazit &quot;zabalené&quot; stránce rozložení. Například, pokud vyberete o odkaz *Views\Home\About.cshtml* je zobrazení vykresleno uvnitř `RenderBody` metody.

Změna záhlaví název webu v šabloně rozložení z &quot;vaše logo&quot; k &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Obsah prvku název nahraďte následující značky:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Spusťte aplikaci a Všimněte si, že nyní říká &quot;MVC Movie &quot;. Klikněte na tlačítko **o** odkaz kde najdete v článku ukazuje, jak tuto stránku &quot;MVC Movie&quot;také. Jsme byli schopni jednou provést změnu v šabloně rozložení a mít všechny stránky na webu odrážely nový název.

![](adding-a-view/_static/image8.png)

Teď Změníme název zobrazení indexu.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Existují dvě místa, kde provést změnu: první, text, který se zobrazí v záhlaví prohlížeče a v hlavičce sekundární ( `<h2>` element). Budete je provedete trochu jinak, uvidíte, jaké verze kódu se změní kterou část aplikace.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

K označení názvu HTML zobrazení kódu nad sady `Title` vlastnost `ViewBag` objektu (což je v *Index.cshtml* zobrazit šablonu). Pokud podíváte zpátky na zdrojový kód šablony rozložení, můžete si všimnout, že šablona používá tuto hodnotu v `<title>` element jako součást `<head>` část HTML, které jsme dřív ke změně. Použití této funkce `ViewBag` přístup, můžete snadno předat další parametry mezi zobrazit šablonu a soubor rozložení.

Spusťte aplikaci a přejděte do `http://localhost:xx/HelloWorld`. Všimněte si, že došlo ke změně názvu prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud se nezobrazí změny v prohlížeči, pravděpodobně jste zobrazili obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči k vynucení odpověď ze serveru, který se má načíst.) Název prohlížeče se vytvoří s `ViewBag.Title` jsme si nastavili *Index.cshtml* zobrazit šablony a další &quot;-filmová aplikace&quot; přidá soubor rozložení.

Všimněte si také způsob, jak v obsahu *Index.cshtml* zobrazit šablonu byl sloučen s  *\_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Rozložení šablony usnadňují skutečně provést změny, které platí pro všechny stránky v aplikaci.

![](adding-a-view/_static/image9.png)

Naše nepatrné &quot;data&quot; (v tomto případě &quot;Hello z našich zobrazit šablonu!&quot; zprávu) je pevně zakódované, i když. Má aplikace MVC &quot;V&quot; (zobrazení) a máte &quot;C&quot; (controller), ale žádné &quot;M&quot; (modelu) ještě. Po chvíli, projdeme postup vytvoření databáze a načíst datový model z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z Kontroleru zobrazení

Předtím, než jsme přejděte k databázi a mluvit o modely, ale nejprve se seznámíme předávání informací z kontroleru zobrazení. Kontroler třídy jsou vyvolány v reakci na příchozí adrese URL žádosti. Třída kontroleru je místo, kde píšete kód, který zpracovává příchozí prohlížeč požádá, načte data z databáze a nakonec rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony pak lze z kontroleru pro generování a formátovat odpověď ve formátu HTML v prohlížeči.

Kontrolery odpovídají za poskytování libovolné dat nebo objekty jsou nutné v pořadí pro šablonu zobrazení k vykreslení odezva do prohlížeče. Osvědčený postup: **zobrazit šablonu by nikdy provádění obchodní logiky nebo pracovat přímo s databází**. Zobrazit šablonu místo toho by měla fungovat jenom s data, která je poskytována kontroleru. Zachování to &quot;oddělení oblastí zájmu&quot; pomáhá udržovat váš kód čistý, možností intenzivního testování a jednodušší údržbu.

V současné době `Welcome` metodu akce v `HelloWorldController` třídy přijímá `name` a `numTimes` parametr a potom výstupy hodnoty přímo do prohlížeče. Spíše než mít řadič vykreslení této odpovědi jako řetězec, Změníme kontroler místo toho použít šablonu zobrazení. Zobrazit šablonu vygeneruje dynamické odpovědi, což znamená, že je potřeba předat odpovídající částí dat z kontroleru zobrazení k vygenerování odpovědi. Můžete to provést tak, že kontroler umístit dynamických dat (parametry), která vyžaduje zobrazení šablony `ViewBag` objekt, který se pak můžou zobrazit šablonu.

Vraťte se na *HelloWorldController.cs* soubor a změňte `Welcome` metoda pro přidání `Message` a `NumTimes` hodnota, která se `ViewBag` objektu. `ViewBag` je to dynamický objekt, což znamená, že můžete vložit cokoliv, co chcete `ViewBag` objekt nemá žádné definované vlastnosti, dokud je něco v ji vložíte. [Systém vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapují pojmenované parametry (`name` a `numTimes`) z řetězce dotazu do adresního řádku parametrům ve své metodě. Kompletní *HelloWorldController.cs* souboru vypadá takto:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nyní `ViewBag` objekt obsahuje data, která bude předána do zobrazení automaticky.

Dále je třeba úvodní zobrazit šablonu! V **sestavení** nabídce vyberte možnost **sestavení MvcMovie** k Ujistěte se, že je projekt kompilován.

Klikněte pravým tlačítkem na uvnitř `Welcome` metoda a klikněte na tlačítko **přidat zobrazení**.

![](adding-a-view/_static/image10.png)

Co **přidat zobrazení** dialogové okno bude vypadat takto:

![](adding-a-view/_static/image11.png)

Klikněte na tlačítko **přidat**a poté přidejte následující kód `<h2>` element na novém *Welcome.cshtml* souboru. Vytvoříte smyčku, která uvádí, že &quot;Hello&quot; tolikrát, kolikrát uživatel uvádí, že by měl. Kompletní *Welcome.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Spusťte aplikaci a přejděte na následující adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data jsou na základě adresy URL a předat pomocí řadiče [modelu pořadače](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Balíčky data do kontroleru `ViewBag` objektu a, které se objekt předá do zobrazení. Zobrazení pak zobrazí data jako kód HTML pro uživatele.

![](adding-a-view/_static/image12.png)

V příkladu výše jsme použili `ViewBag` objekt k předávání dat z kontroleru zobrazení. Druhá možnost v tomto kurzu budeme používat model zobrazení k předání dat z kontroleru zobrazení. Přístup modelu zobrazení k předávání dat je mnohem obecně upřednostňované nad přístup kontejner objektů a dat zobrazení. Naleznete v příspěvku blogu [V silně typované zobrazení dynamické](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Další informace.

Kontejneru, který byl typ z &quot;M&quot; pro model, ale není typ databáze. Pojďme se na to co jsme se naučili a vytvořit databázi videa.

> [!div class="step-by-step"]
> [Předchozí](adding-a-controller.md)
> [další](adding-a-model.md)
