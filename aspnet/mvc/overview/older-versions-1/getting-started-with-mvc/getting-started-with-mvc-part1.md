---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: "Úvod do architektury ASP.NET MVC | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 08c30f4aab77bff64ed3ab874d13cc5dc863fc99
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="intro-to-aspnet-mvc"></a>Úvod do architektury ASP.NET MVC
====================
podle [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Pokud v tomto kurzu je k dispozici aktualizovaná verze [sem](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nový kurz používá ASP.NET MVC 5, který obsahuje mnoho vylepšení v tomto kurzu.
> 
> 
> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


Provedeme naše první webové aplikace ASP.NET MVC pomocí [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Vytočit trochu film seznamu aplikace, která bude dejte nám vytvořit a seznam filmy.

## <a name="what-youll-build"></a>Co budete sestavení

Tady jsou snímky dvě aplikace, kterou budete sestavení. Budete mít jednoduchou tabulku filmy s různé sloupce.

[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

A, musíte vytvořit formulář, filmy jsme můžete přidat do seznamu.

[![Vytvořit film – Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Dovedností, které se dozvíte

V tomto kurzu naučit se základy vytváření webová aplikace ASP.NET MVC pomocí sady Visual Studio. Naučíte:

- Postup vytvoření nového projektu ASP.NET MVC
- Postup vytvoření nové databáze s SQL serverem
- Postup vytvoření zobrazení a Kontrolery architektury ASP.NET MVC
- Jak načíst a zobrazení dat
- Jak upravit data a povolit ověřování dat
- Postup aktualizace schématu databáze

## <a name="get-started"></a>Začínáme

Spusťte spuštěním Visual Web Developer 2010 Express (I budete volání "VWD" od této chvíle) a vyberte nový projekt z obrazovky Start.

Visual Web Developer je IDE, nebo integrované vývojářského prostředí. Stejně jako zápisu dokumentů pomocí aplikace Microsoft Word, použijete k vytvoření aplikací rozhraní IDE. Je panelu nástrojů v horní zobrazuje různé možnosti, které jsou k dispozici je, a také v nabídce může také použili jste vyberte souboru | Nový projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvořit aplikací pomocí jazyka Visual Basic a Visual C#. Nyní, vyberte Visual C# na levé straně pak vyberte "Webová aplikace ASP.NET MVC 2." Název projektu "Filmy" a klikněte na tlačítko OK.

[![Nový projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Na pravé straně je Průzkumník řešení zobrazující všechny soubory a složky v aplikaci. Okno big uprostřed je, kde můžete upravovat kód a většinu doby. Visual Studio použít výchozí šablonu pro projektu ASP.NET MVC, který jste právě vytvořili, takže budete mít funkční aplikaci, ihned bez jakékoli akce! Toto je jednoduchý "Hello, World! projekt a je vhodná pro naši aplikaci spusťte.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Kliknutím na tlačítko "play" na panelu nástrojů.

![Spuštění ladění](getting-started-with-mvc-part1/_static/image11.png)

Je zelenou šipku vpravo, který bude kompilovat vašeho programu a spusťte aplikaci ve webovém prohlížeči.

*Poznámka: Můžete místo toho na klávesnici stiskněte klávesu F5, nebo vybrat ladění -&gt;spustit ladění z nabídky "Debug".*

To způsobí, že Visual Web Developer spustí webový vývoj-server a spuštění webové aplikace (neexistují žádné konfigurace nebo Ruční postup vyžadovaný pro povolení této). Pak se spustí prohlížeč a nakonfigurovat, aby procházet Domovská stránka aplikace. Všimněte si níže, panelu Adresa prohlížeče říká "localhost" a podobný example.com. Je to způsobeno localhost vždycky směřuje na vlastní místní počítač – v takovém případě je spuštěna aplikace, kterou jsme právě vytvořili.

[![Domovská stránka](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Ihned poskytuje tuto výchozí šablonu můžete dvě stránky přejděte a základní přihlašovací stránku. Umožňuje změnit, jak tato aplikace funguje a chvíli informace o architektuře ASP.NET MVC v procesu. Zavřete okno prohlížeče a umožňuje změnit nějaký kód.

>[!div class="step-by-step"]
[Next](getting-started-with-mvc-part2.md)
