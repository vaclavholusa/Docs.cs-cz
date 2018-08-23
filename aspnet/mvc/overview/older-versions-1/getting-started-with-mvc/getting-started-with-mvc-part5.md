---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Přístup k datům modelu z Kontroleru | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752030"
---
<a name="accessing-your-models-data-from-a-controller"></a>Přístup k datům modelu z Kontroleru
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


V této části jsme se chystáte vytvořit novou třídu MoviesController a napsat kód, který načte naše data o filmech a zobrazí ji zpět do prohlížeče pomocí zobrazení šablony.

Klikněte pravým tlačítkem na složku řadiče a proveďte nové MoviesController.

[![Přidání Kontroleru](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Tím se vytvoří nový soubor "MoviesController.cs" pod naše \Controllers složky v rámci naší projektu. Umožňuje aktualizovat MovieController načíst seznam video z naší nově mají údaj vyplněný databáze.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Tak, aby načteme filmy vydanou po léta 1984 jsme provádění dotazu LINQ. Budeme potřebovat zobrazit šablonu pro vykreslení tohoto seznamu filmy zpět, takže klikněte pravým tlačítkem na metodu a vyberte Přidat zobrazení k jeho vytvoření.

V dialogovém okně Přidat zobrazení budete Udáváme, že jsme prochází seznam&lt;Movies.Models.Movie&gt; do našich zobrazení šablony. Na rozdíl od předchozích časy použít dialogové okno Přidat zobrazení jsme se rozhodli vytvořit šablonu služby "Prázdný" tentokrát jsme budete označuje, že chceme, aby Visual Studio automaticky "scaffold" Zobrazit šablonu pro nás s nějakou výchozí obsah. Provedeme to tak, že vyberete "Seznam" položky v rámci "zobrazení obsahu rozevírací nabídky.

Mějte na paměti, když jste vytvořili novou třídu, budete muset kompilovat aplikaci, aby se zobrazí v dialogovém okně Přidat zobrazení.

![Přidání zobrazení](getting-started-with-mvc-part5/_static/image3.png)

Klikněte na tlačítko Přidat a systém automaticky vygeneruje kód pro zobrazení pro USA, který zobrazuje náš seznam videa. Toto je změna vhodná doba &lt;h2&gt; přepravuje do něco jako "Moje film seznam", jako jsme to udělali dříve se zobrazením Hello World.

[![Videa – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Spusťte aplikaci a přejděte do adresního řádku /Movies. Jsme teď načíst data z databáze pomocí základní dotaz do třídy Kontroleru a vrátí data zobrazení, které ví o filmech. Toto zobrazení pak přede prostřednictvím seznamu filmy a vytvoří tabulky dat pro nás.

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Jsme nesmí být implementace úpravy, podrobností a odstraněných funkcí s touto aplikací – proto nepotřebujeme výchozí odkazy, které šablona scaffold vytváří. Otevřete soubor /Movies/Index.aspx a neodeberete.

Zde je zdrojový kód by měl vypadat naše aktualizovanou šablonu zobrazení po jsme tyto změny:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Odkazy, které jsme nebudete potřebovat, se vytváří, je v tomto příkladu odstraníme. Budeme udržovat naše vytvořit nové propojení, jako to je další! Tady je vypadá naši aplikaci pomocí tohoto sloupce odebrány.

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Nyní je k dispozici jednoduchý seznam naše data o filmech. Ale když kliknete na odkaz "Vytvořit nový", jsme chybu, protože získáte ji není připojili! Můžeme implementovat metodu akce vytvoření a povolit tak uživateli zadat nové filmy v databázi.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part4.md)
> [další](getting-started-with-mvc-part6.md)
