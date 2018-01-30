---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: "Přidání sloupce do modelu | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 17ee105f596319423ac83cf718683ed293f952f3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="adding-a-column-to-the-model"></a>Přidání sloupce do modelu
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


V této části přidáme provede jak můžeme provést změny schématu databáze a zpracovat změny v naší aplikaci.

Přidejme do tabulky film slo se "Hodnocení". Vraťte se zpátky a integrovaného vývojového prostředí a kliknutím na Průzkumníka databáze. Klikněte pravým tlačítkem v tabulce film a vyberte otevřete definici tabulky.

Přidá sloupec "Hodnocení", jak vidíte níže. Vzhledem k tomu, že teď nemáme žádné hodnocení, sloupci povolit hodnoty Null. Klikněte na tlačítko Uložit.

[![Úpravy filmy tabulky](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)

V dalším kroku se vrátí do Průzkumníka řešení a otevře soubor Movies.edmx (což je ve složce \Models). Klikněte pravým tlačítkem na návrhovou plochu (bílé oblasti) a vyberte aktualizovat Model z databáze.

[![Filmy - sadu Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)

Tím se spustí Průvodce aktualizací"". Klikněte na kartu aktualizace v něm a klikněte na tlačítko Dokončit. Naše třída modelu film bude potom aktualizovány nového sloupce.

![Průvodce aktualizací (2)](getting-started-with-mvc-part8/_static/image5.png)

Po kliknutí na tlačítko Dokončit, uvidíte, že nový sloupec hodnocení přidala film Entity v našem modelu.

[![Film Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)

Přidali jsme sloupec ve model databáze, ale zobrazení neznáte o něm.

## <a name="update-views-with-model-changes"></a>Aktualizace zobrazení s změny modelu

Aktualizujeme může našich šablon zobrazení tak, aby odrážela nový sloupec hodnocení několika způsoby. Vzhledem k tomu, že jsme vytvořili tato zobrazení vygenerováním pomocí dialogu přidat zobrazení, jsme může odstranit a znovu vytvořte je znovu. Obvykle však osoby bude již provedli změny jejich zobrazení šablon z počáteční vygenerované generování a bude chcete přidat nebo odstranit pole ručně, stejně jako jsme to udělali s pole ID pro vytvoření.

Otevře \Views\Movies\Index.aspx šablony a přidat &lt;tý&gt;hodnocení&lt;/th&gt; k head film tabulky. Po přidání dolování po Genre. Potom ve stejné pozici sloupce, ale nižší dolů, přidejte řádek na výstup naší nové hodnocení.

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

Naše finální šablona Index.aspx bude vypadat takto:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

Umožňuje potom otevře \Views\Movies\Create.aspx šablony a přidat popisek a textové pole pro naše nové vlastnosti hodnocení:

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

Naše finální šablona Create.aspx bude vypadat takto a změňte název a sekundární naše prohlížeče &lt;h2&gt; nápis, který se něco podobného jako "Vytvořit film" při nám sem!

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

Spuštění aplikace a nyní máte k dispozici nové pole v databázi, který je přidán na stránku vytvořit. Přidejte nové film - tentokrát s hodnocení - a klikněte na tlačítko vytvořit.

[![Vytvořit film – Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)

Po kliknutí na tlačítko vytvořit, že posílá indexovou stránku tam, kde můžete nové film je označené nové hodnocení sloupec v databázi.

[![Seznam film – Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)

V tomto kurzu základní tu můžete spustit provedením řadiče a jejich přidružení k zobrazení a předání kolem pevně data. Potom jsme vytvořili a určená databáze a umístí některá data v. Nemůžeme načíst data z databáze a zobrazí do tabulky HTML. Potom jsme přidali vytvořit formulář, který umožní uživateli přidat data do databáze sami z webové aplikace. Jsme přidali ověření a potom provedené ověření pomocí jazyka JavaScript na straně klienta. Nakonec jsme změnit databázi zahrnout nový sloupec dat a potom aktualizovat naše dvě stránky k vytváření a zobrazování tato nová data.

I nyní doporučujeme pokračovat v našem kurzu zprostředkující úrovni "[MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" a také mnoho videa a prostředkům v [https://asp.net/mvc](https://asp.net/mvc) i podrobné informace o architektuře ASP.NET MVC!

Užijte si ji!

- Scott Hanselman - [http://hanselman.com](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru.

>[!div class="step-by-step"]
[Předchozí](getting-started-with-mvc-part7.md)
