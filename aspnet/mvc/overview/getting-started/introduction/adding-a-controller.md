---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Přidávání řadiče | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30869618"
---
<a name="adding-a-controller"></a>Přidání Kontroleru
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Zastupuje rozhraní MVC *model-view-controller*. MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou. Aplikace založené na MVC obsahují:

- **M** odels: třídy, které představují data, aplikace a které používají logiku ověření vynutit obchodní pravidla pro tato data.
- **V** iews: soubory šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.
- **C** ontrollers: načíst data modelu třídy, které zpracovat příchozí požadavky prohlížeče a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.

Jsme budete být pokrývajících tyto koncepty tento kurz řady a ukazují, jak je používat k sestavení aplikace.

> [!NOTE]
> V předchozím kroku MVC výchozí byla zvolena šablona. Tím se vytvoří Domů, účet a Správa řadičů ve výchozím nastavení.

Začněme vytvořením třídy kontroleru. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složku a pak klikněte na tlačítko **přidat**, pak **řadič**.


![](adding-a-controller/_static/image1.png)

V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 – prázdný**a potom klikněte na **přidat**.

![](adding-a-controller/_static/image2.png)  
 

Pojmenujte nový kontroler "HelloWorldController" a klikněte na tlačítko **přidat**.

![Přidání kontroleru](adding-a-controller/_static/image3.png)

Všimněte si v **Průzkumníku řešení** aby nový soubor byla vytvořená s názvem *HelloWorldController.cs* a novou složku *Views\HelloWorld*. Kontroleru je otevřený v prostředí IDE.

![](adding-a-controller/_static/image4.png)

Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontroleru vrátí řetězec HTML jako příklad. Název kontroleru `HelloWorldController` a první metoda jmenuje `Index`. Budeme ji volat z prohlížeče. Spusťte aplikaci (stiskněte F5 nebo Ctrl + F5). V prohlížeči připojit &quot;HelloWorld&quot; na cestu v panelu Adresa. (Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) stránku v prohlížeči bude vypadat jako na následujícím snímku obrazovky. V metodě výše uvedený kód vrátil řetězec přímo. Je systém právě vrátí kód HTML v aplikaci, a to!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC volá jiné řadiče třídy (a různé metody akcí v nich), v závislosti na adresy URL příchozích. Výchozí adresa URL směrování logiku používá ASP.NET MVC používá k určení jaký kód k vyvolání tento formát:

`/[Controller]/[ActionName]/[Parameters]`

Nastavení formátu pro směrování v *aplikace\_Start/RouteConfig.cs* souboru.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Při spuštění aplikace a useanymigrationsubnet všechny segmenty adres URL, výchozí hodnota "Home" kontroleru a metody akce "Index" uvedený v oddílu výchozí hodnoty pro výše uvedený kód.

První část adresy URL určuje třídy kontroleru provést. Proto */HelloWorld* se mapuje `HelloWorldController` třídy. Druhá část adresy URL určuje metody akce v třídě provést. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třída spustí. Všimněte si, že jsme museli vyhledejte */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` je výchozí metodou, která bude volána na řadiči, pokud není explicitně určen. Třetí součást segment adresy URL ( `Parameters`) je pro data trasy. Data trasy, která jsme zobrazí se později v tomto kurzu.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec &quot;metodu úvodní akce... &quot;. Výchozí mapování MVC je `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL, že je řadič `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` součástí ještě adresu URL.

![](adding-a-controller/_static/image6.png)

Pojďme upravit v příkladu mírně tak, aby můžete předat některé informace o parametrech z adresy URL do kontroleru (například */HelloWorld/Vítá? name = Scott&amp;numtimes = 4*). Změna vaší `Welcome` tak, aby zahrnoval dva parametry, jak je uvedeno níže. Všimněte si, že kód používá volitelný parametr funkcí jazyka C# k označení, že `numTimes` parametr by měl výchozí na 1, pokud pro tento parametr není předána žádná hodnota.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Poznámka k zabezpečení: Kód výše používá [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) k ochraně aplikace před zlými úmysly vstup (konkrétně JavaScript). Další informace najdete v části [postupy: ochrana proti skriptu zneužije ve webové aplikaci pomocí použití kódování HTML na řetězce](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Můžete použít různé hodnoty pro `name` a `numtimes` v adrese URL. [Systému vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapuje pojmenované parametry z řetězce dotazu v panelu Adresa parametry ve své metodě.

![](adding-a-controller/_static/image7.png)

V ukázce výše segment adresy URL ( `Parameters`) se nepoužívá, `name` a `numTimes` parametry se jí předávají jako [řetězce dotazu](http://en.wikipedia.org/wiki/Query_string). Na? (otazník) v výše uvedenou adresu URL je oddělovač a postupujte podle řetězce dotazu. &amp; Odděluje řetězce dotazu.

Vítejte metoda nahraďte následujícím kódem:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Spusťte aplikaci a zadejte následující adresu URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Tentokrát třetí segment adresy URL odpovídá parametru trasy `ID.` `Welcome` parametr obsahuje metodu akce (`ID`) odpovídající zadaným specifikace adresy URL v `RegisterRoutes` metoda.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

V aplikacích ASP.NET MVC je obvyklejší předat parametry jako data směrování (jako jsme to udělali s ID výše) než jejich předání jako řetězce dotazu. Můžete také přidat trasu k předání i `name` a `numtimes` v parametrech jako data trasy v adrese URL. V *aplikace\_Start\RouteConfig.cs* soubor, přidejte "text Hello" postup:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Spusťte aplikaci a přejděte do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Výchozí trasu pro mnoho aplikace MVC, funguje bez problémů. Dozvíte později v tomto kurzu k předávání dat pomocí vazače modelu a nebudete mít k úpravě výchozí trasu pro tento.

V těchto příkladech se to řadičem &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práci. Řadičem přímo vrací HTML. Normálně nechcete, aby řadiče vrácení HTML přímo, vzhledem k tomu, který se stane velmi náročná kódu. Místo toho obvykle použijeme oddělená zobrazení souboru šablony ke generování odpovědi HTML. Podíváme se na tom, jak jsme to lze provést další.

> [!div class="step-by-step"]
> [Předchozí](getting-started.md)
> [další](adding-a-view.md)
