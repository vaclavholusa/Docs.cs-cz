---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: "Přístup k vašemu modelu datům z řadiče | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 1a733accabcd10409f5611c31001397e97533fb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller"></a>Přístup k vašemu modelu datům z řadiče
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


V této části jsme se chystáte vytvořit novou třídu MoviesController a napsat kód, naše film data načte a zobrazí zpět do prohlížeče pomocí šablony zobrazení.

Klikněte pravým tlačítkem na složku řadiče a proveďte nové MoviesController.

[![Přidání Kontroleru](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Tím se vytvoří nový soubor "MoviesController.cs" pod naše \Controllers složku v rámci naší projektu. Umožňuje aktualizovat MovieController načíst seznam filmy z našich nově vyplněná databáze.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Dotaz LINQ jsme se provádí tak, aby načteme filmy vydanou po letní 1984. Budeme potřebovat šablonu zobrazení k vykreslení tento seznam filmy zpět, takže klikněte pravým tlačítkem myši v metodě a vyberte možnost Přidat zobrazení k jeho vytvoření.

V dialogovém okně Přidat zobrazení jsme budete znamenat předali seznam&lt;Movies.Models.Movie&gt; do našich zobrazení šablony. Na rozdíl od předchozích dobu jsme používá dialogové okno Přidat zobrazení a rozhodli se vytvořit šablonu "Prázdný" tentokrát jsme budete označuje, že chceme sady Visual Studio automaticky "vygenerovat" Zobrazit šablonu pro nám s některé výchozí obsah. Provedeme to tak, že vyberete "Seznam" položky v rámci "obsahu rozevírací nabídce zobrazení.

Mějte na paměti, když jste vytvořili novou třídu budete potřebovat kompilace aplikace se zobrazí v dialogovém okně Přidat zobrazení.

![Přidání zobrazení](getting-started-with-mvc-part5/_static/image3.png)

Klikněte na tlačítko Přidat a systém automaticky vygeneruje kód pro zobrazení pro nás, které zobrazuje seznam filmy. To je dobrý čas na přechod &lt;h2&gt; záhlaví něco jako "Moje film seznam", jako jsme provedli dříve s zobrazení Hello World.

[![Filmy - sadu Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Spusťte aplikaci a navštivte /Movies na panelu Adresa. Jsme teď načíst data z databáze pomocí základního dotazu v Kontroleru a vrátí data zobrazení, které zná filmy. Toto zobrazení potom otáčí prostřednictvím seznamu filmy a vytvoří tabulku dat pro nás.

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Jsme nebude být implementace úpravy, podrobnosti a odstranění funkce pomocí této aplikace - tak jsme nepotřebujete výchozí odkazy, které vytvořit šablonu vygenerované uživatelské rozhraní pro nás. Otevřete soubor /Movies/Index.aspx a je odebrat.

Tady je zdrojový kód by měl vypadat naše aktualizovanou šablonu zobrazení po jsme proveďte tyto změny:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Vytváří odkazy, které nebudou potřebujeme, takže jsme budete odstranit v tomto příkladu. Ponecháme naše vytvořit nové propojení ale, jak se další! Zde je, jak naše aplikace vypadá se tento sloupec odebrat.

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Nyní je k dispozici jednoduchý seznam naše film data. Ale když kliknete na odkaz "Vytvořit nové", jsme budete dojde k chybě jako ji není připojili! Umožňuje implementovat metodu vytvořit akce a umožní uživateli zadat nové filmy v naší databázi.

>[!div class="step-by-step"]
[Předchozí](getting-started-with-mvc-part4.md)
[další](getting-started-with-mvc-part6.md)
