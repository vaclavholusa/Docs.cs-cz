---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Vytvoření Javascriptového klienta | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b5cb4d93c30ef80a48da48ffc51dd51411b1d0d0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912649"
---
<a name="create-the-javascript-client"></a>Vytvoření Javascriptového klienta
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

V této části vytvoříte klienta pro aplikaci v jazyce HTML, JavaScript a [knihovnou Knockout.js](http://knockoutjs.com/) knihovny. Vytvoříme klientskou aplikaci ve fázích:

- Zobrazuje seznam knihy.
- Zobrazuje podrobnosti adresáře.
- Přidává se nová kniha.

Knihovny Knockout používá vzor Model-View-ViewModel (MVVM):

- **Modelu** je reprezentace dat v doméně business (v našich případu, knihy a autoři) na straně serveru.
- **Zobrazení** je prezentační vrstvy (HTML).
- **Model zobrazení** je objekt jazyka JavaScript obsahující modely. Model zobrazení je abstrakce kód uživatelského rozhraní. Nemá žádné znalosti jazyka HTML představující. Místo toho představuje abstraktní funkce zobrazení, jako například &quot;seznam knihy&quot;.

Zobrazení je vázaný na data do modelu zobrazení. Aktualizace zobrazení modelu se automaticky projeví v zobrazení. Model zobrazení také načte události z zobrazení, jako je tlačítko klikne.

![](part-6/_static/image1.png)

Tento přístup umožňuje snadno změnit rozložení a uživatelského rozhraní aplikace, protože vazby, můžete změnit bez přepsání jakéhokoli kódu. Například může zobrazit seznam položek jako `<ul>`, pak ji později změnit na tabulku.

## <a name="add-the-knockout-library"></a>Přidání knihovny Knockout

V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet**. Potom vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](part-6/samples/sample1.cmd)]

Tento příkaz přidá Knockout soubory do složky skriptů.

## <a name="create-the-view-model"></a>Vytvoření zobrazení modelu

Přidejte soubor JavaScriptu s názvem app.js do složky skriptů. (V Průzkumníku řešení klikněte pravým tlačítkem na složku skripty, vyberte **přidat**a pak vyberte **soubor JavaScript**.) Vložte následující kód:

[!code-javascript[Main](part-6/samples/sample2.js)]

V Knockout `observable` třída umožňuje datové vazby. Při změně obsahu pozorovat, upozorní pozorovat všechny ovládací prvky vázané na data, tak mohou aktualizovat sami. ( `observableArray` Třídy je pole verze *pozorovat*.) Začněte tím náš model zobrazení má dvě pozorovatelné objekty:

- `books` obsahuje seznam knihy.
- `error` obsahuje chybovou zprávu, pokud selže volání AJAX.

`getAllBooks` Metoda provádí volání AJAX k získání seznamu knihy. Pak uloží výsledek do `books` pole.

`ko.applyBindings` Metoda je součástí knihovny Knockout. Model zobrazení jako parametr přijímá a nastaví datovou vazbu.

## <a name="add-a-script-bundle"></a>Přidat sadu skriptů

Sdružování je funkce v technologii ASP.NET 4.5, který umožňuje snadno kombinovat nebo sady více souborů do jediného souboru. Sdružování snižuje počet požadavků na server, což může zlepšit čas načítání stránky.

Otevřete soubor aplikace\_Start/BundleConfig.cs. Přidejte následující kód k metodě RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Předchozí](part-5.md)
> [další](part-7.md)
