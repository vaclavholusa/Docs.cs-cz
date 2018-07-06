---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Přidání ověření do modelu | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: f8172b63fcaa7599f5dd733271d9ea7570e3bcb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802783"
---
<a name="adding-validation-to-the-model"></a>Přidání ověření do modelu
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


V této části budeme implementovat podporu nutná pro povolení ověření vstupu v rámci naší aplikace. Budete zajišťujeme, že náš obsah databáze je vždy správná a koncovým uživatelům poskytovat užitečné chybové zprávy, když se pokusí a zadejte data o filmech, která není platná. Začneme budete přidáním trochu ověřovací logiku do třídy Video.

Klikněte pravým tlačítkem myši klikněte na složku, Model a vyberte Přidat třídu. Zadejte název vaší třídy Video.

Jsme dříve při vytváření modelu Entity filmů, integrovaného vývojového prostředí vytvořit třídu video. Ve skutečnosti součástí film třídy může být v jednom souboru a v další části. Tomu se říká částečnou třídu. Teď ještě chvíli Zůstaneme rozšíření třídy Video z jiného souboru.

Vytvoříme film částečné třídy, na kterou odkazuje na třídu"kamarádské" některé atributy, které se mu ověření systému. Vytvoříme označení názvu a cena podle potřeby a taky trvat, se cena v určitém rozsahu. Klikněte pravým tlačítkem na složku modely a vyberte Přidat třídu. Název vaší třídy filmů a klikněte na tlačítko OK. Zde je, co naše částečné film třídy ve tvaru.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Znovu spusťte aplikaci a zkuste zadat videa s cenou více než 100. Poté, co jste odeslání formuláře budete dojde k chybě. Tato chyba je zachycena na straně serveru a vyvolá se po odeslání formuláře. Všimněte si, jak byly dostatečně inteligentní, aby zobrazení chybové zprávy a Udržovat hodnoty pro nás v rámci textového pole prvků technologie ASP.NET MVC integrovaných pomocných rutin HTML:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

To spolupráce, ale bylo by dobré Pokud jsme mohli říct uživatelům, na straně klienta, hned, předtím, než získá podílejí na serveru.

Umožňuje povolit některé ověřování na straně klienta s použitím jazyka JavaScript.

## <a name="adding-client-side-validation"></a>Přidání ověřování na straně klienta

Vzhledem k tomu, že naše třída Video už má několik atributů ověření, budete potřebujeme přidat několik souborů JavaScriptu do našich Create.aspx zobrazit šablonu a přidejte řádek kódu, které umožňují ověřování na straně klienta, aby proběhla.

V rámci VWD naše zobrazení/video složka go a otevřete Create.aspx.

Otevření složky skriptů v Průzkumníku řešení a přetáhněte následující tři skripty pro v rámci &lt;head&gt; značky.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Chcete tyto soubory skriptu se zobrazí v tomto pořadí.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Přidejte také tento jeden řádek výše Html.BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Tady je kód zobrazený v rámci rozhraní IDE.

[![Videa – Microsoft Visual Web Developer Express 2010 (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Spusťte aplikaci a znovu navštívit /Movies/Create a klikněte na tlačítko vytvořit bez nutnosti zadávat žádná data. Chybové zprávy se zobrazí okamžitě bez flash, přidružené k odesílání dat na stránce všechny způsob, jakým zpět na server. Toto je vzhledem k tomu, že technologie ASP.NET MVC je nyní ověření vstupu u obou klienta (pomocí JavaScriptu) a na serveru.

[![Vytvoření – Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

To je v pořádku. Pojďme nyní přidat jeden další sloupec databáze.

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part6.md)
> [další](getting-started-with-mvc-part8.md)
