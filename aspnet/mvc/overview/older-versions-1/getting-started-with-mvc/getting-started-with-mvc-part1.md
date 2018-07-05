---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Úvod do ASP.NET MVC | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 408256d116a7e73e01c34b0a11881e14c5b8401d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385729"
---
<a name="intro-to-aspnet-mvc"></a>Úvod do ASP.NET MVC
====================
podle [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Pokud v tomto kurzu je k dispozici aktualizovaná verze [tady](../../getting-started/introduction/getting-started.md) pomocí [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Nové kurz používá ASP.NET MVC 5, která nabízí mnoho vylepšení v porovnání s v tomto kurzu.
> 
> 
> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


Vytvoříme naši první webové aplikace ASP.NET MVC pomocí [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Uděláme trochu film aplikaci v seznamu, který bude dejte nám vytvořit a seznamu videa.

## <a name="what-youll-build"></a>Co budete vytvářet

Tady jsou dva snímky obrazovky aplikace, kterou vytvoříte. Budete mít jednoduchou tabulku s filmů s různé sloupce.

[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

A proto jsme do seznamu Přidat videa budete mít formulář vytvořit.

[![Vytvořit aplikaci Windows Internet Explorer (2) film-](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Dovednosti, které se dozvíte

V tomto kurzu se seznámíte se základy vytváření webové aplikace ASP.NET MVC pomocí sady Visual Studio. Získáte informace:

- Jak vytvořit nový projekt ASP.NET MVC
- Jak vytvořit novou databázi s využitím SQL serveru
- Postup vytvoření zobrazení a Kontrolery ASP.NET MVC
- Načtení a zobrazení dat
- Úprava dat a povolení ověření dat
- Postup aktualizace schématu databáze

## <a name="get-started"></a>Začínáme

Začněte tím, že spuštění Visual Web Developer 2010 Express (nazvu to "VWD" od této chvíle) a vyberte nový projekt z obrazovky Start.

Visual Web Developer je integrované vývojové prostředí, nebo integrované prostředí pro vývojáře. Stejným způsobem, jako používáte k tvorbě dokumenty Microsoft Wordu, budete používat integrované vývojové prostředí pro vytváření aplikací. Zde je panel nástrojů podél horního zobrazuje různé možnosti, které jsou k dispozici pro vás, stejně jako nabídka může také použitých pro výběr souboru | Nový projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Vytvoření vaší první aplikace

Můžete vytvářet aplikace pomocí jazyka Visual Basic nebo Visual C#. Teď vyberte Visual C# na levé straně pak vyberte "Webové aplikace ASP.NET MVC 2." Pojmenujte svůj projekt "Filmy" a klikněte na tlačítko OK.

[![Nový projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Na pravé straně se Průzkumník řešení zobrazující všechny soubory a složky ve vaší aplikaci. Kde úpravy kódu a tráví většinu svého času je okno velké objemy uprostřed. Visual Studio použít výchozí šablonu projektu ASP.NET MVC, které jste právě vytvořili, takže Teď máte funkční aplikaci bez teď zrovna nic nedělá! Toto je jednoduchý "Hello World projekt a je vhodné oddělení na zahájení pro naši aplikaci.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Vyberte na panelu nástrojů tlačítko "Přehrát akci".

![Spustit ladění](getting-started-with-mvc-part1/_static/image11.png)

Je zelená šipka směřující doprava, který bude zkompilujete program a spusťte aplikaci ve webovém prohlížeči.

*Poznámka: Můžete místo toho stiskněte klávesu F5 na klávesnici, nebo vybrat ladění –&gt;spustit ladění z nabídky "Ladění".*

To způsobí, že Visual Web Developer ke spuštění webový server vývoje a spuštění webové aplikace (neexistují žádné konfigurace nebo Ruční postup vyžadovaný pro tuto možnost povolte,). Bude potom spusťte prohlížeč a nakonfigurovat, aby procházet Domovská stránka aplikace. Všimněte si, že níže, že na panelu Adresa prohlížeče říká "localhost" a vypadat example.com. Důvodem je, localhost vždy odkazuje na vaše vlastní místní počítače – v tomto případě je spuštěna aplikace, kterou jsme právě vytvořili.

[![Domovská stránka](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Ihned poskytuje tuto výchozí šablonu můžete dvě stránky přejděte a základní přihlašovací stránku. Umožňuje změnit, jak tato aplikace funguje a ještě něco architekturu ASP.NET MVC hlouběji v procesu. Zavřete okno prohlížeče a umožňuje změnit nějaký kód.

> [!div class="step-by-step"]
> [Next](getting-started-with-mvc-part2.md)
