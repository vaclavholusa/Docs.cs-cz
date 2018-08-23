---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Přidání Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d5cc9db7b1eec139a37afb6427fd761342fcc1f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752642"
---
<a name="adding-a-controller"></a>Přidání Kontroleru
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC jsou zahrnovaného *model-view-controller*. MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou. Aplikace využívající architekturu MVC obsahují:

- **M** odels: třídy, které představují data aplikace a logiku ověřování, který slouží k vynucení obchodní pravidla pro tato data.
- **V** iews: souborů šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.
- **C** ontrollers: třídy, které zpracovávají příchozí požadavky prohlížeče, načíst datový model a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.

Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v této řadě kurzů ukazují, jak se dají použít k sestavení aplikace.

> [!NOTE]
> V předchozím kroku MVC výchozí šablona byla vybrána. Domů, vytvoří účet a správa Kontrolérů ve výchozím nastavení.

Začněme tím, že vytvoříte třídu kontroleru. V **Průzkumníka řešení**, klikněte pravým tlačítkem na *řadiče* složku a pak klikněte na tlačítko **přidat**, pak **řadič**.


![](adding-a-controller/_static/image1.png)

V **přidat vygenerované uživatelské rozhraní** dialogové okno, klikněte na tlačítko **kontroler MVC 5 – prázdný**a potom klikněte na tlačítko **přidat**.

![](adding-a-controller/_static/image2.png)  
 

Pojmenujte nový kontroler "HelloWorldController" a klikněte na tlačítko **přidat**.

![Přidání kontroleru](adding-a-controller/_static/image3.png)

Všimněte si, že v **Průzkumníka řešení** zda nový soubor byl vytvořen pojmenovaný *HelloWorldController.cs* a novou složku *Views\HelloWorld*. Kontroler je otevřen v integrovaném vývojovém prostředí.

![](adding-a-controller/_static/image4.png)

Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontroleru vrátí řetězec HTML jako příklad. Název kontroleru `HelloWorldController` a první metoda se jmenuje `Index`. Pojďme ho vyvolat z prohlížeče. Spusťte aplikaci (stisknutím klávesy F5 nebo Ctrl + F5). V prohlížeči, připojte &quot;HelloWorld&quot; na cestu v panelu Adresa. (Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) na stránce v prohlížeči bude vypadat jako na následujícím snímku obrazovky. Ve výše uvedené metody kód vrátil řetězec přímo. Jste uvedli jako systém právě vrátí kód HTML a udělal!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC vyvolá jiný kontroler třídy (a různé metody akcí v nich) v závislosti na příchozí adrese URL. Výchozí adresy URL směrování logiky používá ASP.NET MVC používá k určení jaký kód, který má být vyvolán formátu tímto způsobem:

`/[Controller]/[ActionName]/[Parameters]`

Nastavení formátu pro směrování *aplikace\_Start/RouteConfig.cs* souboru.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Při spuštění aplikace a nechcete zadat všechny segmenty adres URL, použije se výchozí "Domovskou" kontroleru a metody akce "Index" definováno v sekci výchozí hodnoty z výše uvedený kód.

První část adresy URL určuje třída kontroleru k provedení. Takže */HelloWorld* mapuje `HelloWorldController` třídy. Druhá část adresy URL určí metodu akce v třídě ke spuštění. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění. Všimněte si, že jsme museli procházet */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` představuje výchozí metodu, která bude volána na řadiči, pokud není explicitně zadaná. Třetí část segment adresy URL ( `Parameters`) je pro data trasy. Uvidíme data trasy, která dále v tomto kurzu.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec &quot;Toto je metoda úvodní akce... &quot;. Výchozí mapování MVC `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` část adresy URL ještě.

![](adding-a-controller/_static/image6.png)

Pojďme mírně upravte příklad tak, aby některé informace o parametrech z adresy URL můžete předat do kontroleru (například */HelloWorld/uvítací? název = Scott&amp;numtimes = 4*). Změna vašeho `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno níže. Všimněte si, že kód používá volitelný parametr funkce jazyka C# k označení, že `numTimes` parametr by ve výchozím nastavení 1-li pro tento parametr není předána žádná hodnota.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Poznámka k zabezpečení: Kódu nad používá [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) k ochraně aplikace před zlými úmysly vstup (konkrétně JavaScript). Další informace najdete v části [postupy: ochrana proti skript zneužije ve webové aplikaci pomocí použitím kódování HTML na řetězce](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Můžete vyzkoušet různé hodnoty pro `name` a `numtimes` v adrese URL. [Systém vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapují pojmenované parametry z řetězce dotazu do adresního řádku parametrům ve své metodě.

![](adding-a-controller/_static/image7.png)

V ukázce výše, segment adresy URL ( `Parameters`) se nepoužívá, `name` a `numTimes` parametry jsou předány jako [řetězce dotazu](http://en.wikipedia.org/wiki/Query_string). Na? (otazník) v adrese URL výše je oddělovač a postupujte podle řetězce dotazu. &amp; Odděluje řetězce dotazu.

Úvodní metoda nahraďte následujícím kódem:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Spusťte aplikaci a zadejte následující adresu URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Tentokrát třetí segment adresy URL odpovídající parametr trasa `ID.` `Welcome` metody akce obsahuje parametr (`ID`), který odpovídá specifikaci adresy URL v `RegisterRoutes` metody.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

V aplikacích ASP.NET MVC je obvyklejší a zajistěte tak předání parametrů jako směrování dat (jak jsme to udělali s ID výše), než byly předány jako řetězce dotazu. Můžete také přidat trasu k předání i `name` a `numtimes` v parametrech jako data trasy v adrese URL. V *aplikace\_Start\RouteConfig.cs* přidejte trasy "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Spusťte aplikaci a přejděte do `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Výchozí trasu pro mnoho aplikací MVC funguje správně. Dozvíte později v tomto kurzu k předávání dat pomocí vazače modelu a nebudete už muset upravit výchozí trasy pro daný.

V těchto příkladech je to kontroleru &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práce. Kontroler přímo vrací HTML. Obvykle nechcete řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód. Místo toho obvykle použijeme soubor šablony samostatným zobrazením ke generování odpovědi HTML. Pojďme se podívat na tom, jak to můžeme udělat.

> [!div class="step-by-step"]
> [Předchozí](getting-started.md)
> [další](adding-a-view.md)
