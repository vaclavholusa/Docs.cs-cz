---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "Přidání ověřování do modelu | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 25c939bc8121589f91914e553d56e8f0975115b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a>Přidání ověřování do modelu
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


V této části přidáme implementovat podporu nutná pro povolení ověření vstupu v naší aplikaci. Jsme budete zajistěte, aby náš obsah databáze je vždy správný a koncovým uživatelům poskytovat užitečné chybové zprávy, když se nepovede, zadejte film data, která není platná. Začneme budete přidáním trochu logiku ověření pro třídu film.

Klikněte pravým tlačítkem na složku, Model a vyberte Přidat třídu. Název vaší třídy film.

Pokud jsme vytvořili Model Entity film předtím rozhraní IDE vytvořit třídu film. Ve skutečnosti součástí třídy film může být v jednom souboru a část v jiném. Tomu se říká konkrétní třídu. Vytvoříme rozšíření třídy film z jiného souboru.

Vytvoříme třídu částečné film který odkazuje na třídu"kamarád" s některé atributy, které se mu dávat pokyny ověření do systému. Jsme budete označit nadpis a cenu podle požadavku a taky trvat, být do určitého rozsahu. Klikněte pravým tlačítkem na složku modely a vyberte Přidat třídu. Název vaší třídy film a klikněte na tlačítko OK. Zde je co naše částečné třídy vypadá film jako.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Znovu spusťte aplikaci a zkuste zadejte film s cenou více než 100. Poté, co jste odeslání formuláře budete dojde k chybě. Chyba je zachycena na straně serveru a vyskytne se po odeslání formuláře. Všimněte si, jak byly ASP.NET MVC předdefinované pomocné rutiny HTML dostatečně inteligentní, zobrazí se chybová zpráva a Udržovat hodnoty pro nám v rámci prvků textového pole:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

To vyhovující postup, ale bude dobrý, jsme může informace pro uživatele na straně klienta, hned, než získá podílejí na server.

Umožňuje povolit některé ověřování na straně klienta v jazyce JavaScript.

## <a name="adding-client-side-validation"></a>Přidání ověřování na straně klienta

Vzhledem k tomu, že naše třída film již některých atributů ověření, budete potřebujeme přidejte několik souborů JavaScript pro naše Create.aspx zobrazit šablonu a přidejte řádek kódu povolit ověřování na straně klienta proběhla.

Z přejděte v rámci VWD naše složky zobrazení/film a otevře Create.aspx.

Otevře složky skriptů v Průzkumníku řešení a přetáhněte ji následující tři skriptů v rámci &lt;head&gt; značky.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Chcete tyto soubory skriptu, než se objeví v tomto pořadí.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Navíc přidejte tento jeden řádek výše Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Tady je kód uvedené v rámci rozhraní IDE.

[![Filmy - sadu Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Spusťte aplikaci a znovu přejděte /Movies/Create a klikněte na tlačítko vytvořit bez nutnosti zadávat žádná data. Chybové zprávy, které se zobrazí okamžitě bez stránce flash, že jsme přidružit k odesílání dat všechny způsob zpět na server. Toto je, protože rozhraní ASP.NET MVC je nyní ověření vstupu na obou klienta (pomocí jazyka JavaScript) a na serveru.

[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

To je vyhledávání dobrý! Přidejme jeden další sloupec teď do databáze.

>[!div class="step-by-step"]
[Předchozí](getting-started-with-mvc-part6.md)
[další](getting-started-with-mvc-part8.md)
