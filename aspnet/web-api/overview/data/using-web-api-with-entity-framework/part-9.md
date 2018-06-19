---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Přidat novou položku do databáze | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868370"
---
<a name="add-a-new-item-to-the-database"></a>Přidat novou položku do databáze
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

V této části bude přidána možnost pro uživatele k vytvoření nového seznamu. V app.js přidejte následující kód do modelu zobrazení:

[!code-javascript[Main](part-9/samples/sample1.js)]

V Index.cshtml vyměňte následující kód:

[!code-html[Main](part-9/samples/sample2.html)]

Pomocí:

[!code-html[Main](part-9/samples/sample3.html)]

Tento kód vytvoří formulář pro odeslání nové autora. Hodnoty pro rozevíracího seznamu Autor jsou vázané na data do `authors` pozorovatelné v zobrazení modelu. Pro ostatní vstupy formuláře, hodnoty jsou vázané na data do `newBook` vlastnost modelu zobrazení.

Obslužná rutina odeslání formuláře je vázána `addBook` funkce:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` Funkce načte aktuální hodnoty vázané na data formuláře vstupy pro vytvoření objekt JSON. Pak se odešle objekt JSON, který má `/api/books`.

> [!div class="step-by-step"]
> [Předchozí](part-8.md)
> [další](part-10.md)
