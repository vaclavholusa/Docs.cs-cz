---
title: "Přidání zobrazení do aplikace MVC"
author: Rick-Anderson
description: "Přidání zobrazení do aplikace MVC"
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 86887f0dafa31ff3eb6597284c469c4b3053b6b7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-view"></a>Přidání zobrazení
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

V této části se chystáte upravit `HelloWorldController` třídu se má použít zobrazení soubory šablon do této aplikace zapouzdření proces generování odpovědi HTML pro klienta. 

Vytvoříte zobrazení šablony souboru pomocí [zobrazovací modul Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Zobrazení syntaxe Razor pro šablony mají *.cshtml* příponu souboru a poskytují elegantní způsob vytvoření HTML výstup pomocí jazyka C#. Syntaxe Razor minimalizuje počet znaků a požadované při zápisu zobrazit šablonu stisknutí kláves a umožňuje rychlé, kapaliny kódování pracovního postupu.

Aktuálně `Index` metoda vrátí řetězec s zprávu, která je pevně zakódovaná ve třídě controller. Změna `Index` metoda vrátí `View` objektu, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

`Index` Metody popsané výše používá šablonu zobrazení pro generování odpovědi HTML do prohlížeče. Metody kontroleru (také označované jako [metody akce](http://rachelappel.com/asp.net-mvc-actionresults-explained)), například `Index` metody popsané výše, obecně vrátit [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (nebo třídy odvozené od [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), není primitivní typy, jako řetězec.

Klikněte pravým tlačítkem *Views\HelloWorld* složky a klikněte na tlačítko **přidat**, pak klikněte na tlačítko **stránka zobrazení MVC 5 s rozložením (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
V **zadejte název položky** dialogovém okně zadejte *Index*a potom klikněte na **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
V **vybrat stránku rozložení** dialogové okno, přijměte výchozí nastavení  **\_Layout.cshtml** a klikněte na tlačítko **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
V dialogovém okně výše *Views\Shared* je vybrána složka v levém podokně. Pokud jste měli soubor vlastní rozložení v jiné složce, může ho vyberte. Budeme mluvit o rozložení souboru později v tomto kurzu

*MvcMovie\Views\HelloWorld\Index.cshtml* soubor je vytvořen.

![](adding-a-view/_static/image4.png)

Přidejte následující zvýrazněný kód.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Klikněte pravým tlačítkem *Index.cshtml* soubor a vyberte **zobrazit v prohlížeči**.

![PLATFORMY](adding-a-view/_static/image5.png)

Můžete také pravým tlačítkem *Index.cshtml* soubor a vyberte **zobrazení v nástroj Page Inspector.** Najdete v článku [nástroj Page Inspector kurzu](../../views/using-page-inspector-in-aspnet-mvc.md) Další informace.

Alternativně spusťte aplikaci a přejděte do `HelloWorld` řadiče (`http://localhost:xxxx/HelloWorld`). `Index` Metoda do kontroleru nebyla práci mnohem; jednoduše byl spuštěn příkaz `return View()`, které určené, že metoda by měla používat soubor šablony zobrazení k vykreslení odpovědi do prohlížeče. Vzhledem k tomu, že nebyla výslovně zadat název souboru šablony zobrazení pro použití, ASP.NET MVC uvedena pomocí *Index.cshtml* zobrazení souboru v *\Views\HelloWorld* složky. Následující obrázek ukazuje řetězec &quot;Hello z našich zobrazit šablonu!&quot; pevně v zobrazení.

![](adding-a-view/_static/image6.png)

Vypadá poměrně funkční. Všimněte si však, že záhlaví prohlížeče ukazuje &quot;Index - Moje aplika ASP.NET "a odkaz velká nahoře na stránce říká"Název aplikace." V závislosti na tom, jak malého provedete okna prohlížeče, možná budete muset klikněte na tři řádky v pravé horní zobrazíte do **Domů**, **o**, **kontaktujte**, **Zaregistrovat** a **přihlásit** odkazy.

## <a name="changing-views-and-layout-pages"></a>Změna zobrazení a rozložení stránky

První, kterou chcete změnit &quot;název aplikace&quot; odkaz v horní části stránky. Tento text je běžné na každou stránku. Jsou ve skutečnosti implementované na jenom jednom místě v projektu, i když se zobrazí na každé stránce v aplikaci. Přejděte na */zobrazení/sdílené* složky v **Průzkumníku řešení** a otevřete  *\_Layout.cshtml* souboru. Tento soubor se nazývá *rozložení stránky* a je ve sdílené složce, použít všechny ostatní stránky.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Šablony rozložení umožňují zadat rozložení kontejneru HTML vašeho webu na jednom místě a pak ho použít na více stránkách ve vaší lokalitě. Najít `@RenderBody()` řádku. `RenderBody`je zástupný symbol kde všechny na zobrazení konkrétní stránky, můžete vytvořit zobrazení, &quot;zabalené&quot; na stránce rozložení. Například, pokud jste vybrali **o** odkaz, *Views\Home\About.cshtml* zobrazení vykresleno uvnitř `RenderBody` metoda.

Změňte obsah elementu název. Změna [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) v šabloně rozložení z &quot;název aplikace&quot; k &quot;MVC film&quot; a kontroler, z `Home` k `Movies`. Dokončení rozložení souboru je zobrazena níže:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Spusťte aplikaci a Všimněte si, že teď uvádí &quot;MVC film &quot;. Klikněte **o** odkaz a v tématu jak na této stránce se zobrazí &quot;MVC film&quot;, příliš. Jsme byli schopni jednou proveďte změny v šabloně rozložení a mít všechny stránky webu odrážet nový název.

![](adding-a-view/_static/image8.png)

Když jsme nejdřív vytvořili *Views\HelloWorld\Index.cshtml* souboru obsahuje následující kód:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Kódu Razor výše je výslovně nastavení ke stránce rozložení. Zkontrolujte *zobrazení\\soubor _ViewStart.cshtml* souboru, obsahuje kód přesně stejnou Razor. *[Zobrazení\\soubor _ViewStart.cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)*  soubor definuje běžné rozložení, který bude používat všechna zobrazení, proto můžete okomentovat out nebo odeberte tento kód z *Views\HelloWorld\ Index.cshtml* souboru.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Můžete použít `Layout` vlastnost nastavit různé rozložení zobrazení, nebo ji nastavte na `null` , použije se žádný soubor rozložení.

Nyní změňte název zobrazení pro Index.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Existují dvě místa změnit: nejdřív, zobrazí se text v názvu prohlížeče a pak v hlavičce sekundární ( `<h2>` element). Budete je provedete mírně lišit, abyste viděli, které bit kódu změní kterou částí aplikace.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

K označení HTML nadpis můžete zobrazit, kód výše sady `Title` vlastnost `ViewBag` objektu (se v *Index.cshtml* zobrazit šablonu). Všimněte si, že šablona rozložení ( *Views\Shared\\_Layout.cshtml* ) používá tuto hodnotu v `<title>` element jako součást `<head>` části HTML, které jsme dříve upravit.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Pomocí této `ViewBag` přístup, můžete snadno předat další parametry mezi zobrazit šablonu a rozložení souboru.

Spusťte aplikaci. Všimněte si, že došlo ke změně záhlaví prohlížeče, záhlaví primární a sekundární záhlaví. (Pokud nevidíte změny v prohlížeči, může být zobrazil obsah uložený v mezipaměti. Stisknutím kláves Ctrl + F5 v prohlížeči na vynucení odpovědi ze serveru načíst.) Záhlaví prohlížeče je vytvořena s `ViewBag.Title` nastaví *Index.cshtml* zobrazit šablony a další &quot;-filmová aplikace&quot; přidat v souboru rozložení.

Všimněte si také jak obsah *Index.cshtml* zobrazit šablonu se sloučil s  *\_Layout.cshtml* zobrazit šablonu a odpověď o jedné HTML byl odeslán do prohlížeče. Šablony rozložení skutečně usnadňují provést změny, které platí pro všechny stránky v aplikaci.

![](adding-a-view/_static/image9.png)

Naše trocha &quot;data&quot; (v tomto případě &quot;Hello z našich zobrazení šablony!&quot; zpráva) je pevně zakódovaná, přestože. Má aplikace MVC &quot;V&quot; (zobrazení) a máte k dispozici &quot;C&quot; (řadič), ale žádné &quot;M&quot; (modelu) ještě. Zanedlouho projdeme postup vytvoření databáze a načíst data modelu z něj.

## <a name="passing-data-from-the-controller-to-the-view"></a>Předání dat z řadiče zobrazení

Před jsme přejděte k databázi a mluvit o modely, ale umožňuje nejprve mluvit o předání informací z řadiče zobrazení. Třídy kontroleru je vyvolán v odpovědi na požadavek příchozí adresy URL. Třída kontroleru je, kde můžete psát kód, který zpracovává příchozí prohlížeče požadavky, načte data z databáze a nakonec rozhodne, jaký typ odpověď k odeslání zpět do prohlížeče. Zobrazit šablony pak lze z řadiče pro vygenerování a formátování odpovědi HTML do prohlížeče.

Řadiče jsou zajišťuje ať data nebo objekty jsou potřeba v pořadí pro šablonu zobrazení k vykreslení odpovědi do prohlížeče. Osvědčeným postupem: **by měla zobrazit šablonu nikdy provádět obchodní logiku nebo pracovat přímo s databází**. Místo toho by měla zobrazit šablonu fungovat jenom s data, která je k němu poskytované řadičem. Zachování to &quot;oddělené oblasti zájmu&quot; pomáhá udržovat kódu čistá, možností intenzivního testování a více udržovatelný.

V současné době `Welcome` metodu akce v `HelloWorldController` třídy trvá `name` a `numTimes` parametr a potom výstupy hodnoty přímo do prohlížeče. Místo mít řadič vykreslení této odpovědi jako řetězec, umožňuje změnit řadič místo toho použít šablonu zobrazení. Zobrazit šablonu způsobí vygenerování dynamických odpovědí, která znamená, že potřebujete předat příslušné bits dat z řadiče zobrazení za účelem vygenerování odpovědi. To provedete tak, že řadiče put dynamických dat (parametry), která vyžaduje zobrazení šablony `ViewBag` objekt, který šablona zobrazení můžete poté přistoupit.

Vraťte se do *HelloWorldController.cs* soubor a změňte `Welcome` metody přidat `Message` a `NumTimes` hodnotu `ViewBag` objektu. `ViewBag`je to dynamický objekt, což znamená, že všechno můžete vložit do ní; `ViewBag` objekt nemá žádné definované vlastnosti, dokud vložíte něco uvnitř ho. [Systému vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapuje pojmenované parametry (`name` a `numTimes`) z řetězce dotazu v panelu Adresa parametry ve své metodě. Kompletní *HelloWorldController.cs* soubor vypadá takto:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Nyní `ViewBag` objekt obsahuje data, která bude předána do zobrazení automaticky. Dále je nutné zadat šablonu úvodní zobrazení! V **sestavení** nabídce vyberte možnost **sestavit řešení** (nebo Ctrl + Shift + B) a ujistěte se, kompilace projektu. Klikněte pravým tlačítkem *Views\HelloWorld* složky a klikněte na tlačítko **přidat**, pak klikněte na tlačítko **stránka zobrazení MVC 5 s rozložením (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
V **zadejte název položky** dialogovém okně zadejte *úvodní*a potom klikněte na **OK**.   
  
V **vybrat stránku rozložení** dialogové okno, přijměte výchozí nastavení  **\_Layout.cshtml** a klikněte na tlačítko **OK**.  
  
![](adding-a-view/_static/image11.png)   

*MvcMovie\Views\HelloWorld\Welcome.cshtml* soubor je vytvořen.

Nahraďte kód v *Welcome.cshtml* souboru. Vytvoříte smyčku, která uvádí, že &quot;Hello&quot; tolikrát, kolikrát uživatel zvolí by měl. Kompletní *Welcome.cshtml* souboru je uveden níže.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Spusťte aplikaci a přejděte na následující adresu URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Data jsou převzaty z adresy URL a předaný pomocí řadiče [vazač modelu](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Balíčky data do kontroleru `ViewBag` objekt a předává, které objektu do zobrazení. Zobrazení pak zobrazí data jako kód HTML pro uživatele.

![](adding-a-view/_static/image12.png)

V ukázce výše jsme použili `ViewBag` objekt, který chcete předat data z řadiče zobrazení. Později v tomto kurzu budeme používat model zobrazení k předávání dat z řadiče zobrazení. Zobrazení modelu přístupu k předávání dat je obvykle mnohem upřednostňované nad přístup kontejner objektů a dat zobrazení. Najdete v položkách blogu [dynamická V silného typu zobrazení](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) Další informace. 

Také, který byl typ služby &quot;M&quot; modelu, ale není typ databáze. Podívejme se, co jsme jste se naučili a vytvořit databázi filmy.

>[!div class="step-by-step"]
[Předchozí](adding-a-controller.md)
[další](adding-a-model.md)
