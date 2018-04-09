---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Vytvoření klienta JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 29d50e448e6d282c7db06b9d1946ac221347e1ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="create-the-javascript-client"></a>Vytvoření klienta JavaScript
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

V této části vytvoříte klienta pro aplikaci, pomocí HTML, JavaScript a [Knockout.js](http://knockoutjs.com/) knihovny. Klientská aplikace jsme budete vytvářet ve fázích:

- Zobrazuje seznam seznamů.
- Zobrazuje podrobnosti adresáře.
- Přidání nového seznamu.

Knihovny Knockout využívá schéma Model-View-ViewModel (modelem MVVM):

- **Modelu** je serverové reprezentace dat v doméně obchodní (v našem případ, knihy a autoři).
- **Zobrazení** je prezentační vrstvy (HTML).
- **Modelu zobrazení** se objekt jazyka JavaScript, která obsahuje modely. Model zobrazení je abstrakce kód uživatelského rozhraní. Nemá žádné znalosti reprezentace HTML. Místo toho představuje abstraktní zobrazení, funkce, jako &quot;seznam seznamů&quot;.

Zobrazení je vázané na data do modelu zobrazení. Aktualizace do modelu zobrazení, se automaticky promítnou v zobrazení. Model zobrazení také získá události ze zobrazení, například kliknutím na tlačítko.

![](part-6/_static/image1.png)

Tento přístup umožňuje snadno změnit rozložení a uživatelského rozhraní aplikace, protože vazby, můžete změnit bez přepisování žádný kód. Například může zobrazit seznam položek, jako `<ul>`, poté ji později změnit na tabulku.

## <a name="add-the-knockout-library"></a>Přidání knihovny Knockout

V sadě Visual Studio z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**. Potom vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](part-6/samples/sample1.cmd)]

Tento příkaz přidá Knockout soubory do složky skriptů.

## <a name="create-the-view-model"></a>Vytvoření modelu zobrazení

Přidejte soubor JavaScript s názvem app.js do složky skriptů. (V Průzkumníku řešení klikněte pravým tlačítkem na složku skripty, vyberte **přidat**, pak vyberte **soubor JavaScript**.) Vložte následující kód:

[!code-javascript[Main](part-6/samples/sample2.js)]

V Knockout `observable` třída umožňuje datové vazby. Při změně obsahu existuje zjištěný, upozorní lze zobrazit všechny ovládací prvky vázané na data, mohou sami aktualizovat. ( `observableArray` Třída je pole verze *lze zobrazit*.) Naše model zobrazení začínat, má dva pozorovatelné objekty:

- `books` obsahuje seznam knih.
- `error` obsahuje chybovou zprávu, pokud selže volání AJAX.

`getAllBooks` Metoda provede volání AJAX získat seznam knih. Pak vynutí výsledek do `books` pole.

`ko.applyBindings` Metoda je součástí knihovny Knockout. Přebírá jako parametr modelu zobrazení a nastaví datové vazby.

## <a name="add-a-script-bundle"></a>Přidat sady skriptu

Sdružování je funkce v technologii ASP.NET 4.5, která usnadňuje kombinovat nebo sady více souborů do jediného souboru. Sdružování snižuje počet požadavků na server, což může zlepšit čas načítání stránky.

Otevřete soubor aplikace\_Start/BundleConfig.cs. Přidejte následující kód do metody RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Předchozí](part-5.md)
> [další](part-7.md)
