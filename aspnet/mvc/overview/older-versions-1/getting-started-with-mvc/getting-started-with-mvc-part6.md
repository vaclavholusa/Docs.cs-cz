---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Přidávání metody Create a vytvořit zobrazení | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: cf0d721b551c38e8c38e35f82b73ee1b14cd068f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840963"
---
<a name="adding-a-create-method-and-create-view"></a>Přidávání metody Create a zobrazení Create
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


V této části budeme implementovat podporu nutná pro povolení uživatelům vytvářet nové filmy v databázi. Provedeme to pomocí implementace akce URL videa nebo vytvoření.

Implementace adresu URL videa nebo vytvoření provádí dva kroky. Když uživatel navštíví první adresa URL videa nebo vytvoření chceme, aby se budou zobrazovat formuláře HTML, který můžete vyplnit zadat nový film. Potom když uživatel odešle formulář opravdu zavřít a příspěvky, které data zpět na server, chcete načíst odeslaných obsah a uložte ho do databáze.

Tyto dva kroky v rámci dvou metod Create() jsme budete implementovat v rámci naší MoviesController třídy. Zobrazí se jedné metody &lt;formuláře&gt; , uživatel by měl vyplnit k vytvoření nové video. Druhá metoda zajistí zpracování odeslaných dat, když uživatel odešle &lt;formuláře&gt; zpět na server a uložit nový film v rámci naší databázi.

Níže je kód přidáme k naší MoviesController třídu pro implementaci toto:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Ve výše uvedeném kódu obsahuje kód, který budeme ho potřebovat v rámci Kontroleru.

Pojďme teď implementovat šablony vytvořit zobrazení, který použijeme k zobrazení formuláře pro uživatele. Jsme budete klikněte pravým tlačítkem myši v první metodě vytvoření a vyberte "Přidat zobrazení" Vytvoření šablony zobrazení pro naše film formuláře.

Vybereme, které jsme se, že přejdete na zobrazení šabloně předat "Video" jako jeho třída zobrazení dat a označuje, že chceme "generování uživatelského rozhraní" "Vytvořit" šablonu.

[![Přidání zobrazení](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Po kliknutí na tlačítko Přidat, vytvoří se pro vás \Movies\Create.aspx zobrazit šablonu. Protože jsme vybrali "Vytváření" z rozevíracího seznamu "Zobrazit obsah", dialogové okno Přidat zobrazení automaticky "vygenerovanou" některé výchozí obsah pro nás. Vytvoří základní kostry aplikace HTML &lt;formuláře&gt;, místo, kde chyba ověření zprávy přejít, a protože generování uživatelského rozhraní ví o filmech, vytvoří pro každou vlastnost Naše třída popisek a pole.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Vzhledem k tomu, že naše databáze automaticky poskytuje filmu ID, Odebereme těchto polí tohoto referenčního modelu. ID z našich zobrazení pro vytváření. Odebrat 7 řádky za &lt;legendy&gt;pole&lt;/legend&gt; budou předvádět pole ID, nechceme.

Pojďme teď vytvořit nový film a přidejte ho do databáze. Budeme to provést spuštěním aplikaci znovu spustit a přejděte "/ filmy" adresa URL a klepnutím na odkaz Přidat nové video "Vytvořit".

[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Když kliknete na tlačítko vytvořit, jsme budete mít účtování zpět (přes HTTP POST) data na tomto formuláři /Movies/Create metodu, kterou jsme právě vytvořili. Stejně jako při systém automaticky trvalo "numTimes" a "name" parametr z adresy URL a mapovat na parametry pro metodu dříve bude systém automaticky trvat, než pole formuláře POST a jejich namapování na objekt. V tomto případě hodnoty z polí v HTML, jako třeba "ReleaseDate" a "Title" automaticky zařadí se do správné vlastnosti novou instanci třídy videa.

Podívejme se na druhý způsob vytvoření z našich MoviesController znovu. Všimněte si, jak ho přebírá objekt "Video" jako argument:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Toto video bylo předáno pak [HttpPost] verzi metodě akce vytvořit, a My uloženo v databázi a pak uživatel přesměrován zpět na metodu akce Index(), kde uložený výsledek se zobrazí v seznamu video:

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Jsme nejsou kontrolu, pokud naše videa jsou správné, i když a databázi neumožní nám uložit videa s bez názvu. Bylo by dobré, pokud jsme mohli říct uživatelům, který před databáze došlo k chybě. Provedeme dále přidáním podpory ověřování pro naši aplikaci.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part5.md)
> [další](getting-started-with-mvc-part7.md)
