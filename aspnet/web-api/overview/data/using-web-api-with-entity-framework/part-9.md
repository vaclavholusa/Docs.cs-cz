---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Přidat novou položku do databáze | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: b1f7935c70efcc3ee486e76fc356ff43716632dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368874"
---
<a name="add-a-new-item-to-the-database"></a>Přidat novou položku do databáze
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

V této části bude přidána možnost pro uživatele k vytvoření nového adresáře. V app.js přidejte následující kód k zobrazení modelu:

[!code-javascript[Main](part-9/samples/sample1.js)]

V Index.cshtml nahraďte následující značky:

[!code-html[Main](part-9/samples/sample2.html)]

Pomocí:

[!code-html[Main](part-9/samples/sample3.html)]

Tento kód vytvoří formulář pro odeslání nového Autor. Hodnoty pro autora rozevíracího seznamu je vázán na data `authors` pozorovatelných v modelu zobrazení. Pro ostatní vstupy formuláře, hodnoty jsou vázán na data `newBook` vlastnost modelu zobrazení.

Obslužné rutiny odeslání ve formuláři je vázán na `addBook` funkce:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` Funkce přečte aktuální hodnoty vázané na data formuláře vstupy pro vytvoření objektu JSON. Pak odešle objekt JSON, který má `/api/books`.

> [!div class="step-by-step"]
> [Předchozí](part-8.md)
> [další](part-10.md)
