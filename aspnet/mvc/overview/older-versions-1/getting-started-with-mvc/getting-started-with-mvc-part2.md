---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Přidání Kontroleru | Dokumentace Microsoftu
author: shanselman
description: Najít aktualizovanou verzi, pokud v tomto kurzu je k dispozici zde prostřednictvím sady Visual Studio 2013. Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 856b7d98ac8bd30982d81b0609bb9c1288e07e49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754821"
---
<a name="adding-a-controller"></a>Přidání Kontroleru
====================
podle [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Pokud v tomto kurzu je k dispozici aktualizovaná verze [tady](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s v tomto kurzu.
> 
> 
> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


MVC jsou zahrnovaného Model, zobrazení a kontroler. MVC je vzor pro vývoj aplikací tak, aby měla každá část odpovědnosti, který se liší od jiného.

- Model: Data vaší aplikace
- Zobrazení: Soubory šablon, které vaše aplikace bude používat k dynamickému generování odpovědi HTML.
- Řadiče: Třídy, které zpracovávají příchozí žádosti adresy URL pro aplikaci, načíst datový model a pak zadejte zobrazit šablony, které vykreslují odpověď zpět do klienta

Společnost Microsoft a budete moct pokrývající všechny tyto koncepty v tomto kurzu ukazují, jak se dají použít k sestavení aplikace.

Ve složce řadiče v Průzkumníku řešení pravým tlačítkem myši a vyberete přidat kontroler vytvoříme nový kontroler.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Pojmenujte nový kontroler "HelloWorldController" a klikněte na tlačítko Přidat.

[![Přidat Dialog Kontroleru](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Všimněte si, že v Průzkumníku řešení na pravé straně, který byl vytvořen nový soubor pro jste volali HelloWorldController.cs a tento soubor je teď otevřený v **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Vytvořte dvě nové metody, které v rámci vaší novou veřejnou třídu HelloWorldController vypadat nějak takto. Vrátí řetězec HTML přímo z kontroleru jako příklad.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Kontrolér jmenuje HelloWorldController a nová metoda se nazývá Index. Spusťte aplikaci znovu, stejně jako v minulosti (klikněte na tlačítko Přehrát akci nebo stiskněte klávesu F5, můžete to udělat). Po zahájení prohlížeči změnit cestu v panelu Adresa `http://localhost:xx/HelloWorld` kde xx je cokoli, co číslo počítače zvolil. Váš prohlížeč teď by měl vypadat jako na následujícím snímku obrazovky. V našem výše uvedené metody jsme vrátí řetězec předán metodu nazvanou "Obsah". Řekli jsme systém právě vrátí kód HTML a udělal!

ASP.NET MVC volá různé třídy Kontroleru (a různé metody akce v nich) v závislosti na příchozí adrese URL. Logika výchozí mapování používá ASP.NET MVC používá formát takto řídit, jaký kód běží:

/ [Kontroler] / [název akce] / [parametry]

První část adresy URL určuje třída Kontroleru k provedení. Proto /HelloWorld mapuje HelloWorldController třídy. Druhá část adresy URL určí metodu akce v třídě ke spuštění. Proto /HelloWorld/Index by způsobila metoda Index() třídy HelloWorldcontroller ke spuštění. Všimněte si, že jsme měli jen k navštívení /HelloWorld výše a metodu, kterou Index byl odvozen. Je to proto, že metodu s názvem "Index", je výchozí metodou, která bude volána na řadiči, pokud není explicitně zadaná.

[![Toto je Moje výchozí akce](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Teď se podívejme se na `http://localhost:xx/HelloWorld/Welcome.` teď naše úvodní metoda má spuštěn a vrátí jeho řetězec ve formátu HTML.

Znovu nebo [kontroler] / [název akce] / [parametry] tak Kontroleru je HelloWorld a Vítejte v tomto případě je metoda. Zatím jsme dosud neučinili parametry.

[![Toto je metoda úvodní akce](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Pojďme upravit naše ukázka mírně tak, aby se dají předat některé informace v z adresy URL do kontroleru, třeba takto: / HelloWorld/uvítací? název = Scott&amp;numtimes = 4. Změníte úvodní metodu dva parametry a aktualizace je podobná níže uvedenému příkladu. Všimněte si, že jsme použili nepovinný parametr funkce jazyka C# k označení, že numTimes parametr by ve výchozím nastavení 1 Pokud není předán v.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Spusťte aplikaci a navštivte `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` změna hodnoty názvu a numtimes, jak potřebujete. Systém automaticky mapují pojmenované parametry v řetězci dotazu do adresního řádku parametrům ve své metodě.

V obou těchto příkladech kontroleru bylo to všechnu práci a má se vrácení HTML přímo. Obvykle bychom naše řadiče vrácení HTML přímo – protože je velmi náročná kód, který končí. Místo toho jsme budete obvykle použijte samostatný soubor šablony zobrazení účelem vygenerování odpovědi HTML. Podívejme se na tom, jak to můžeme udělat. Zavřete okno prohlížeče a vraťte se do integrovaného vývojového prostředí.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part1.md)
> [další](getting-started-with-mvc-part3.md)
