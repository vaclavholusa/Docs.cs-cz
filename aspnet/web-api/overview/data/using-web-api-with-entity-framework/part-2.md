---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Přidání modelů a Kontrolerů | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 162ef2cd4ba11040e1bc938617a36495489ba5bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752850"
---
<a name="add-models-and-controllers"></a>Přidání modelů a Kontrolerů
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

V této části přidáte tříd modelu, které definují entity databáze. Potom přidáte kontrolerů rozhraní Web API, které provádějí operace CRUD s těmito entitami.

## <a name="add-model-classes"></a>Přidání třídy modelu

V tomto kurzu vytvoříme databázi s použitím "Code First" přístup do Entity Framework (EF). S Code First třídy jazyka C#, které odpovídají zapíšete do databázové tabulky a EF vytvoří databázi. (Další informace najdete v tématu [vývojové přístupy s Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Začneme tak, že definujete naše objektů domény jako POCOs (prostý staré CLR objekty). Vytvoříme POCOs následující:

- Autor
- Knihy

V Průzkumníku řešení klikněte pravým tlačítkem myši klikněte na složku modely. Vyberte **přidat**a pak vyberte **třídy**. Název třídy `Author`.

![](part-2/_static/image1.png)

Často používaný kód v Author.cs nahraďte následujícím kódem.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Přidejte další třídu s názvem `Book`, následujícím kódem.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework pomocí těchto modelů vytvoření databázových tabulek. Pro každý model `Id` vlastnost se stane sloupec primárního klíče tabulky databáze.

Ve třídě adresáře `AuthorId` definuje cizí klíč do `Author` tabulky. (Pro jednoduchost, můžu jsem za předpokladu, že jednotlivé knihy má jeden Autor.) Kniha třída také obsahuje vlastnosti navigace použít k související `Author`. Můžete použít navigační vlastnost pro přístup k související `Author` v kódu. Další informace o vlastnosti navigace v části 4, řeknu [zpracování relací prvků](part-4.md).

## <a name="add-web-api-controllers"></a>Přidat Kontrolerů webového rozhraní API

V této části přidáme kontrolerů rozhraní Web API, které podporují operace CRUD (vytváření, čtení, aktualizace a odstranění). Kontrolery pomocí Entity Framework komunikovat s databázové vrstvě.

Nejprve můžete odstranit soubor Controllers/ValuesController.cs. Tento soubor obsahuje kontroler Web API příklad, ale nepotřebujete ho pro účely tohoto kurzu.

![](part-2/_static/image2.png)

V dalším kroku sestavení projektu. Generování uživatelského rozhraní webového rozhraní API používá reflexi k nalezení tříd modelu, proto musí zkompilovaného sestavení.

V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče. Vyberte **přidat**a pak vyberte **řadič**.

![](part-2/_static/image3.png)

V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte možnost "webového rozhraní API 2 kontroler s akcemi používající nástroj Entity Framework". Klikněte na tlačítko **přidat**.

![](part-2/_static/image4.png)

V **přidat kontroler** dialogového okna, postupujte takto:

1. V **třída modelu** rozevírací seznam, vyberte `Author` třídy. (Pokud se nezobrazuje v rozevíracím seznamu nezobrazí, ujistěte se, že jste sestavili projekt.)
2. Zkontrolujte "Použití asynchronní akce kontroleru".
3. Nechte název tak řadič jako &quot;AuthorsController&quot;.
4. Klikněte na tlačítko plus (+) vedle tlačítka **třída kontextu dat**.

![](part-2/_static/image5.png)

V **nový kontext dat.** dialogového okna, ponechte výchozí název a klikněte na **přidat**.

![](part-2/_static/image6.png)

Klikněte na tlačítko **přidat** Dokončit **přidat kontroler** dialogového okna. Dialog přidá dvě třídy do projektu:

- `AuthorsController` Definuje kontroler Web API. Kontroler implementuje rozhraní REST API, kterou klienti používají k provádění operací CRUD u seznamu autoři.
- `BookServiceContext` Spravuje objekty entity za běhu, který obsahuje naplnění objekty s daty z databáze, řešení change tracking a zachovává data do databáze. Dědí z `DbContext`.

![](part-2/_static/image7.png)

V tomto okamžiku projekt znovu sestavte. Nyní absolvovat stejný postup, chcete-li přidat kontroler API pro `Book` entity. Tentokrát vyberte `Book` pro třídu modelu a vyberte existující `BookServiceContext` třída pro třídy datového kontextu. (Nevytvářejte nový kontext dat.) Klikněte na tlačítko **přidat** přidat kontroler.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Předchozí](part-1.md)
> [další](part-3.md)
