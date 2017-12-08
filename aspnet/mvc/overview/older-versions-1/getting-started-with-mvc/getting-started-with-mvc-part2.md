---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: "Přidávání řadiče | Microsoft Docs"
author: shanselman
description: "Aktualizovaná verze, pokud v tomto kurzu je k dispozici zde pomocí sady Visual Studio 2013. Nový kurz používá ASP.NET MVC 5, který nabízí mnoho vylepšení v porovnání s t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 93a362cf83d39b29fcba3f2dee0c28257805a89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Přidání Kontroleru
====================
podle [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Pokud v tomto kurzu je k dispozici aktualizovaná verze [sem](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nový kurz používá ASP.NET MVC 5, který obsahuje mnoho vylepšení v tomto kurzu.
> 
> 
> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


MVC je zkratka pro Model, zobrazení, řadiče. MVC je vzor pro vývoj aplikací tak, aby měl každý část odpovědnost, které se liší od jiného.

- Model: Data aplikace
- Zobrazení: Soubory šablon vaše aplikace bude používat k dynamickému generování odpovědi HTML.
- Řadiče: Třídy, které zpracovávat příchozí požadavky na adresu URL pro aplikaci, načíst data modelu a potom zadejte zobrazit šablony, které vykreslení odpověď zpět do klienta

Budete se o tyto koncepty v tomto kurzu jsme ukazují, jak je používat k sestavení aplikace.

Umožňuje vytvořit nový řadič kliknutím pravým tlačítkem na složku řadiče v Průzkumníku řešení a výběrem možnosti Přidat kontroler.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Pojmenujte nový kontroler "HelloWorldController" a klikněte na tlačítko Přidat.

[![Dialogové okno řadiče přidání](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Všimněte si, v Průzkumníku řešení na pravé straně, který byl vytvořen nový soubor můžete volat HelloWorldController.cs a tento soubor je nyní otevřít v **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Vytvořte dvě nové metody, které vypadají takto uvnitř novou veřejnou třídu HelloWorldController. Vrátí řetězec HTML přímo z našich řadiče jako příklad.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Řadiči jmenuje HelloWorldController a nová metoda je volána Index. Spusťte aplikaci znovu, stejně jako předtím (klikněte na tlačítko Přehrát akci nebo stiskněte klávesu F5 k tomu). Jakmile je zahájen prohlížeč, změnit cestu v panelu Adresa `http://localhost:xx/HelloWorld` zvolil kde xx je ať číslo počítače. Váš prohlížeč by měl nyní vypadat jako na následující snímek obrazovky. V našem metody popsané výše jsme vrátí řetězec předaný do metodu s názvem "Obsahu." Můžeme vás vyzval systém právě vrátí kód HTML, a to!

ASP.NET MVC volá různé třídy Controller (a různé metody akce v nich), v závislosti na adresy URL příchozích. Logika výchozí mapování používá ASP.NET MVC používá k řízení, jaký kód běží tento formát:

/ [Controller] / [název akce] nebo [parametry]

První část adresy URL určuje třídy Kontroleru provést. Proto /HelloWorld mapuje k třídě HelloWorldController. Druhá část adresy URL určuje metody akce v třídě provést. Proto /HelloWorld/Index by způsobilo metodu Index() třídy HelloWorldcontroller provést. Všimněte si, že jsme pouze museli navštívit /HelloWorld výše a metodu, kterou byla implicitní Index. Je to proto, že je metoda s názvem "Index" výchozí metoda, která bude volána na řadiči, pokud není explicitně určen.

[![Toto je můj výchozí akci.](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Nyní Pojďme navštivte `http://localhost:xx/HelloWorld/Welcome.` teď naše úvodní metoda má provést a vrátí jeho řetězec HTML.

Znovu nebo [Controller] / [název akce] nebo [parametry] tak, aby je řadič HelloWorld a Vítá v tomto případě je metoda. Zatím jsme dosud neučinili parametry.

[![Toto je metoda úvodní akce](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Pojďme upravit naše ukázka mírně tak, aby některé informace v z adresy URL můžete předat do kontroleru, například takto: / HelloWorld/Vítá? name = Scott&amp;numtimes = 4. Změňte metodu úvodní zahrnuje dva parametry a aktualizace je třeba níže. Upozorňujeme, že jsme si volitelný parametr funkcí jazyka C# k označení, že numTimes parametr by měl výchozí na 1, pokud není předán v.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Spusťte aplikaci a navštivte `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` změna hodnoty názvu a numtimes, jak se vám líbí. Systém automaticky mapují pojmenované parametry z řetězec vašeho dotazu na panelu Adresa parametry ve své metodě.

V obou těchto příkladech řadičem byl všechny pracuje a má byla vrácení HTML přímo. Normálně Neradi bychom naše řadiče vrácení HTML přímo - vzhledem k tomu, který bude mít je velmi náročná kódu. Místo toho obvykle použijeme samostatný soubor šablony zobrazení ke generování odpovědi HTML. Podíváme, jak jsme to můžete provést. Zavřete prohlížeč a vraťte se k prostředí IDE.

>[!div class="step-by-step"]
[Předchozí](getting-started-with-mvc-part1.md)
[další](getting-started-with-mvc-part3.md)
