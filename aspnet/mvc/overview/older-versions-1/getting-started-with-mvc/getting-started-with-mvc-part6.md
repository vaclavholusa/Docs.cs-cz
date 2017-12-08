---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: "Přidávání vytváření metoda a vytvořit zobrazení | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 4792689087ab85be25fe186b2ec97915af448ef9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-create-method-and-create-view"></a>Přidávání vytváření metoda a vytvořit zobrazení
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


V této části přidáme implementovat podporu nutná pro povolení uživatelům vytvářet nové filmy v naší databázi. Provedeme to implementací filmy nebo vytvořit adresu URL akce.

Implementace filmy nebo vytvoření adresy URL je dvou krocích. Pokud uživatel navštíví první adresa URL filmy nebo vytvořit chceme je zobrazovat formuláře HTML, který může vyplnit k zadání nového film. Pak když uživatel odešle formulář a příspěvcích zpět na server, chceme načtení odeslaných obsah a jeho uložení do databáze.

Tyto dva kroky v rámci dvou metod Create() jsme budete implementovat v rámci naší MoviesController třídy. Zobrazí jednu metodu &lt;formuláře&gt; , uživatel by měl vyplnit k vytvoření nové film. Druhá metoda zpracovává zpracování odeslaných dat, když uživatel odešle &lt;formuláře&gt; zpět na server a uložte nový film v rámci naší databáze.

Níže je kód přidáme do našich MoviesController třídu pro implementaci toto:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

Výše uvedený kód obsahuje všechny kód, který budeme potřebovat v rámci Kontroleru.

Teď umožňuje implementovat šablony vytvořit zobrazení, která použijeme k zobrazení formuláře pro uživatele. Jsme budete klikněte pravým tlačítkem v první metodu Create a vyberte možnost "Přidat zobrazení" pro vytvoření šablony zobrazení pro naše film formulář.

Jsme vyberete, které jsme se chystáte předat zobrazit šablonu "Film" jako třída jeho zobrazení dat a znamenat, že má "vygenerovat" šablonu "Vytvořit".

[![Přidání zobrazení](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Po kliknutí na tlačítko Přidat, vytvoří se pro vás \Movies\Create.aspx zobrazit šablonu. Vzhledem k tomu, že jsme vybrali "Vytvořit" z rozevíracího seznamu "zobrazení obsahu", dialogové okno Přidat zobrazení automaticky "vygeneroval" některé výchozí obsah pro nás. Generování uživatelského rozhraní vytvořit HTML &lt;formuláře&gt;, místo pro chybu ověřování zpráv přejdete a vzhledem k tomu, že generování uživatelského rozhraní ví o filmy, vytvoření popisku a pole pro každou vlastnost naše třídy.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Vzhledem k tomu, že naše databáze automaticky poskytuje film ID, odeberte umožňuje těchto polí tohoto referenčního modelu. ID z našich zobrazení pro vytváření. Odeberte 7 řádky po &lt;legendy&gt;pole&lt;/legend&gt; jak ukazují pole ID jsme nechcete.

Umožňuje nyní vytvořte nové film a přidejte ho do databáze. Jsme budete Uděláte to tak, že spustíte aplikaci znovu a přejděte "nebo filmy" adresa URL a klikněte na tlačítko "Vytvořit" propojit přidání nové videa.

[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Když kliknete na tlačítko pro vytvoření, jsme budete mít publikování zpět (přes HTTP POST) data na tomto formuláři metodu /Movies/Create, který jsme právě vytvořili. Stejně jako při systém automaticky trvalo parametr "numTimes" a "název" mimo adresu URL a mapovat je na parametry pro metodu dříve bude systém automaticky trvat pole formuláře POST a jejich namapování na objekt. V takovém případě hodnot polí ve formátu HTML jako "ReleaseDate" a "Title" bude automaticky přepnut do správné vlastnosti novou instanci třídy film.

Podívejme se na druhé metody vytvořit z našich MoviesController znovu. Všimněte si, jak dlouho trvá objekt "Film" jako argument:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Tento objekt film byla předána do verze [HttpPost] naše vytvořit metody akce a jsme uloženy v databázi a potom přesměruje uživatele zpět na Index() metody akce, které se zobrazí uložený výsledek v seznamu video:

[![Seznam film – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

Jsme nejsou kontrolu, pokud naše filmy, když jsou správné, a databáze nebude povolovat nám uložit film s bez názvu. Je dobrý Pokud jsme může informace pro uživatele, který před databáze došlo k chybě. Tato další jsme to udělat tak, že přidáte podporu ověřování do aplikace.

>[!div class="step-by-step"]
[Předchozí](getting-started-with-mvc-part5.md)
[další](getting-started-with-mvc-part7.md)
