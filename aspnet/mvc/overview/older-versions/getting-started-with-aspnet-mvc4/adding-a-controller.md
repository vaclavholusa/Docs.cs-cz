---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Přidání Kontroleru | Dokumentace Microsoftu
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici tady, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, sledovat a ukázka mnohem jednodušší...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 772f3c87d6b73b324164bf9619d332a9c01d7476
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756408"
---
<a name="adding-a-controller"></a>Přidání Kontroleru
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


MVC jsou zahrnovaného *model-view-controller*. MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou. Aplikace využívající architekturu MVC obsahují:

- **M** odels: třídy, které představují data aplikace a logiku ověřování, který slouží k vynucení obchodní pravidla pro tato data.
- **V** iews: souborů šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.
- **C** ontrollers: třídy, které zpracovávají příchozí požadavky prohlížeče, načíst datový model a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.

Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v této řadě kurzů ukazují, jak se dají použít k sestavení aplikace.

Začněme tím, že vytvoříte třídu kontroleru. V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *řadiče* složku a pak vyberte **přidat kontroler**.

![](adding-a-controller/_static/image1.png)

Pojmenujte nový kontroler &quot;HelloWorldController&quot;. Ponechte výchozí šablony jako **prázdný kontroler MVC** a klikněte na tlačítko **přidat**.

![Přidání kontroleru](adding-a-controller/_static/image2.png)

Všimněte si, že v **Průzkumníka řešení** zda nový soubor byl vytvořen pojmenovaný *HelloWorldController.cs*. Soubor je otevřen v integrovaném vývojovém prostředí.

![](adding-a-controller/_static/image3.png)

Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontroleru vrátí řetězec HTML jako příklad. Název kontroleru `HelloWorldController` a se jmenuje první výše uvedené metody `Index`. Pojďme ho vyvolat z prohlížeče. Spusťte aplikaci (stisknutím klávesy F5 nebo Ctrl + F5). V prohlížeči, připojte &quot;HelloWorld&quot; na cestu v panelu Adresa. (Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) na stránce v prohlížeči bude vypadat jako na následujícím snímku obrazovky. Ve výše uvedené metody kód vrátil řetězec přímo. Jste uvedli jako systém právě vrátí kód HTML a udělal!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC vyvolá jiný kontroler třídy (a různé metody akcí v nich) v závislosti na příchozí adrese URL. Výchozí adresy URL směrování logiky používá ASP.NET MVC používá k určení jaký kód, který má být vyvolán formátu tímto způsobem:

`/[Controller]/[ActionName]/[Parameters]`

První část adresy URL určuje třída kontroleru k provedení. Takže */HelloWorld* mapuje `HelloWorldController` třídy. Druhá část adresy URL určí metodu akce v třídě ke spuštění. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třídy ke spuštění. Všimněte si, že jsme museli procházet */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` představuje výchozí metodu, která bude volána na řadiči, pokud není explicitně zadaná.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec &quot;Toto je metoda úvodní akce... &quot;. Výchozí mapování MVC `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL kontroleru je `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` část adresy URL ještě.

![](adding-a-controller/_static/image5.png)

Pojďme mírně upravte příklad tak, aby některé informace o parametrech z adresy URL můžete předat do kontroleru (například */HelloWorld/uvítací? název = Scott&amp;numtimes = 4*). Změna vašeho `Welcome` tak, aby zahrnoval dva parametry, jak je znázorněno níže. Všimněte si, že kód používá volitelný parametr funkce jazyka C# k označení, že `numTimes` parametr by ve výchozím nastavení 1-li pro tento parametr není předána žádná hodnota.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Můžete vyzkoušet různé hodnoty pro `name` a `numtimes` v adrese URL. [Systém vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapují pojmenované parametry z řetězce dotazu do adresního řádku parametrům ve své metodě.

![](adding-a-controller/_static/image6.png)

V obou těchto příkladech je to kontroleru &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práce. Kontroler přímo vrací HTML. Obvykle nechcete řadiče vrácení HTML přímo, protože, který se stane velmi náročný kód. Místo toho obvykle použijeme soubor šablony samostatným zobrazením ke generování odpovědi HTML. Pojďme se podívat na tom, jak to můžeme udělat.

> [!div class="step-by-step"]
> [Předchozí](intro-to-aspnet-mvc-4.md)
> [další](adding-a-view.md)
