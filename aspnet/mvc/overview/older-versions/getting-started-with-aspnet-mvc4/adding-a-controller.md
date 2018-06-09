---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Přidávání řadiče | Microsoft Docs
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: bb76c0a87d935322406b9d8e18fbdb3e41f327f5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30868864"
---
<a name="adding-a-controller"></a>Přidání Kontroleru
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


Zastupuje rozhraní MVC *model-view-controller*. MVC je vzor pro vývoj aplikací, které jsou dobře navrženou, možností intenzivního testování a snadnou údržbou. Aplikace založené na MVC obsahují:

- **M** odels: třídy, které představují data, aplikace a které používají logiku ověření vynutit obchodní pravidla pro tato data.
- **V** iews: soubory šablon, které vaše aplikace používá k dynamickému generování odpovědi HTML.
- **C** ontrollers: načíst data modelu třídy, které zpracovat příchozí požadavky prohlížeče a pak zadejte zobrazit šablony, které vracejí odezva do prohlížeče.

Jsme budete být pokrývajících tyto koncepty tento kurz řady a ukazují, jak je používat k sestavení aplikace.

Začněme vytvořením třídy kontroleru. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složku a potom vyberte **přidat kontroler**.

![](adding-a-controller/_static/image1.png)

Pojmenujte nový kontroler &quot;HelloWorldController&quot;. Ponechte výchozí šablony jako **kontroler MVC prázdný** a klikněte na tlačítko **přidat**.

![Přidání kontroleru](adding-a-controller/_static/image2.png)

Všimněte si v **Průzkumníku řešení** aby nový soubor byla vytvořená s názvem *HelloWorldController.cs*. Soubor je otevřen v prostředí IDE.

![](adding-a-controller/_static/image3.png)

Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Metody kontroleru vrátí řetězec HTML jako příklad. Název kontroleru `HelloWorldController` a první metoda výše jmenuje `Index`. Budeme ji volat z prohlížeče. Spusťte aplikaci (stiskněte F5 nebo Ctrl + F5). V prohlížeči připojit &quot;HelloWorld&quot; na cestu v panelu Adresa. (Například na obrázku níže, jeho `http://localhost:1234/HelloWorld.`) stránku v prohlížeči bude vypadat jako na následujícím snímku obrazovky. V metodě výše uvedený kód vrátil řetězec přímo. Je systém právě vrátí kód HTML v aplikaci, a to!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC volá jiné řadiče třídy (a různé metody akcí v nich), v závislosti na adresy URL příchozích. Výchozí adresa URL směrování logiku používá ASP.NET MVC používá k určení jaký kód k vyvolání tento formát:

`/[Controller]/[ActionName]/[Parameters]`

První část adresy URL určuje třídy kontroleru provést. Proto */HelloWorld* se mapuje `HelloWorldController` třídy. Druhá část adresy URL určuje metody akce v třídě provést. Proto */HelloWorld/Index* by způsobilo `Index` metodu `HelloWorldController` třída spustí. Všimněte si, že jsme museli vyhledejte */HelloWorld* a `Index` metoda byla použita ve výchozím nastavení. Důvodem je, že metodu s názvem `Index` je výchozí metodou, která bude volána na řadiči, pokud není explicitně určen.

Přejděte do `http://localhost:xxxx/HelloWorld/Welcome`. `Welcome` Metoda spustí a vrátí řetězec &quot;metodu úvodní akce... &quot;. Výchozí mapování MVC je `/[Controller]/[ActionName]/[Parameters]`. Pro tuto adresu URL, že je řadič `HelloWorld` a `Welcome` je metoda akce. Nebyly použity `[Parameters]` součástí ještě adresu URL.

![](adding-a-controller/_static/image5.png)

Pojďme upravit v příkladu mírně tak, aby můžete předat některé informace o parametrech z adresy URL do kontroleru (například */HelloWorld/Vítá? name = Scott&amp;numtimes = 4*). Změna vaší `Welcome` tak, aby zahrnoval dva parametry, jak je uvedeno níže. Všimněte si, že kód používá volitelný parametr funkcí jazyka C# k označení, že `numTimes` parametr by měl výchozí na 1, pokud pro tento parametr není předána žádná hodnota.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Spusťte aplikaci a přejděte na adresu URL příklad (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Můžete použít různé hodnoty pro `name` a `numtimes` v adrese URL. [Systému vazby modelu ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automaticky mapuje pojmenované parametry z řetězce dotazu v panelu Adresa parametry ve své metodě.

![](adding-a-controller/_static/image6.png)

V obou těchto příkladech se to řadičem &quot;VC&quot; část MVC – to znamená, zobrazení a kontroler práci. Řadičem přímo vrací HTML. Normálně nechcete, aby řadiče vrácení HTML přímo, vzhledem k tomu, který se stane velmi náročná kódu. Místo toho obvykle použijeme oddělená zobrazení souboru šablony ke generování odpovědi HTML. Podíváme se na tom, jak jsme to lze provést další.

> [!div class="step-by-step"]
> [Předchozí](intro-to-aspnet-mvc-4.md)
> [další](adding-a-view.md)
