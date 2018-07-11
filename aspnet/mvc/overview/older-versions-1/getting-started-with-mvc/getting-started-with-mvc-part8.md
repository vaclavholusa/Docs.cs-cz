---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Přidání sloupce do modelu | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 5dbda1280c073d5d4f2d71918ca31bc44475f64d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817198"
---
<a name="adding-a-column-to-the-model"></a>Přidání sloupce do modelu
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


V této části jsme se chystáte projít jak můžeme provádět změny schématu databáze a zpracovat změny v rámci naší aplikace.

Přidejme do tabulky Movie slo se "Hodnocení". Přejděte zpět do integrovaného vývojového prostředí a kliknutím na Průzkumníka databáze. Tabulky Movie klikněte pravým tlačítkem a vyberte Otevřít definici tabulky.

Přidáte sloupec "Hodnocení", jak je vidět níže. Vzhledem k tomu, že teď nemáme žádné hodnocení, sloupec můžete povolit hodnoty Null. Klikněte na Uložit.

[![Úpravy filmy tabulky](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

V dalším kroku vrátit do okna Průzkumník řešení a otevře soubor Movies.edmx (která je ve složce \Models). Klikněte pravým tlačítkem myši na návrhové ploše (bílé oblasti) a vyberte aktualizace modelů z databáze.

[![Videa – Microsoft Visual Web Developer Express 2010 (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Tím se spustí Průvodce aktualizací"". Klikněte na kartu aktualizace v něm a klikněte na tlačítko Dokončit. Naše třída modelu film pak aktualizuje s nový sloupec.

![Průvodce aktualizací (2)](getting-started-with-mvc-part8/_static/image5.png)

Po kliknutí na tlačítko Dokončit, uvidíte, že do Entity filmu v náš model se přidal nový sloupec hodnocení.

[![Video Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Přidali jsme sloupec v modelu databáze, ale zobrazení nevíte o něm.

## <a name="update-views-with-model-changes"></a>Aktualizace zobrazení s změny modelu

Existuje několik způsobů, jak může aktualizujeme naše šablony zobrazení tak, aby odrážely nový sloupec hodnocení. Protože jsme vytvořili tato zobrazení vygenerováním přes dialogové okno Přidat zobrazení, jsme může je odstranit a znovu je vytvořte znovu. Obvykle ale lidé budou už provedli změny k jejich zobrazení šablony z generování počáteční vygenerované a budete chtít přidat nebo odstranit pole ručně, stejně jako jsme to udělali s poli ID pro vytvoření.

Otevřete \Views\Movies\Index.aspx šablonu a přidejte &lt;th&gt;hodnocení&lt;/th&gt; k začátku tabulky Movie. Po přidání dolovat po žánr. Potom na stejné pozici sloupce, ale níže, přidejte řádek pro výstup naší nové hodnocení.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Naše finální šablona Index.aspx bude vypadat například takto:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Pojďme potom otevřete \Views\Movies\Create.aspx šablonu a přidejte popisek a textové pole pro naše nové vlastnosti hodnocení:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Naše finální šablona Create.aspx bude vypadat nějak takto a změňme náš prohlížeč title a sekundární &lt;h2&gt; název na něco jako "Vytvořit video" Když jsme tady!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Spusťte aplikaci a teď máte v databázi, který se přidal na stránku vytvořit nové pole. Přidání nového videa – tentokrát s hodnocení – a klikněte na tlačítko vytvořit.

[![Vytvořit aplikaci Windows Internet Explorer film-](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Po kliknutí na vytvořit přecházíte na indexovou stránku tam, kde jste nové video je uvedený s nového hodnocení sloupce v databázi

[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

V tomto kurzu základní teď vám pomůže začít vytváření Kontrolerů, jejich přidružení k zobrazení a předáním kolem údajů o pevně zakódované. Potom jsme vytvořili určená databáze a vložte některá data v. Jsme načíst data z databáze a zobrazí ho v tabulku HTML. Potom jsme přidali vytvořit formulář, který umožní uživateli přidat data do databáze sami z v rámci webové aplikace. Jsme přidali ověřování a poté provedli ověření pomocí jazyka JavaScript na straně klienta. Nakonec jsme změnit databáze, kterou chcete zahrnout nový sloupec dat a potom aktualizovat naše dvě stránky, které umožňuje vytvořit a zobrazit tato nová data.

Nyní neváhejte se přejít na náš kurz pro středně "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" a také mnoho videa a materiály v [ https://asp.net/mvc ](https://asp.net/mvc) získat i další informace o ASP.NET MVC!

Užijte si!

- Scott Hanselman – [ http://hanselman.com ](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part7.md)
