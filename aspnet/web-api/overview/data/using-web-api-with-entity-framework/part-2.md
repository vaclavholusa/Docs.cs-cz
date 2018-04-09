---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Přidat modely a řadičů | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a>Přidání řadiče a modely
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

V této části přidáte třídy modelu, které definují entity databáze. Potom přidáte řadiče webového rozhraní API, které provádění operací CRUD tyto entity.

## <a name="add-model-classes"></a>Přidání třídy modelu

V tomto kurzu vytvoříme databázi pomocí "Code First" přístup k Entity Framework (EF). S Code First zápisu třídy jazyka C#, které odpovídají tabulkách databáze a EF vytvoří databázi. (Další informace najdete v tématu [Entity Framework vývoj přístupy](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Začneme definováním naše objektů domény jako POCOs (prostý starý CLR objekty). Vytvoříme POCOs následující:

- Autor
- Adresáře

V Průzkumníku řešení klikněte pravým tlačítkem složku modely. Vyberte **přidat**, pak vyberte **třída**. Název třídy `Author`.

![](part-2/_static/image1.png)

Všechny standardní kód Author.cs nahraďte následujícím kódem.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Přidejte jinou třídu s názvem `Book`, s následujícím kódem.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Rozhraní Entity Framework použije tyto modely k vytvoření databázové tabulky. Pro každý model `Id` vlastnost bude sloupec primárního klíče tabulky databáze.

Ve třídě adresáře `AuthorId` definuje cizí klíč do `Author` tabulky. (Pro jednoduchost, I mě za předpokladu, že každý seznam má jeden Autor.) Třída kniha také obsahuje vlastnost navigace, která má související `Author`. Navigační vlastnost lze použít pro přístup k související `Author` v kódu. Další informace o navigačních vlastností v rámci 4, říci [zpracování vztahy entit](part-4.md).

## <a name="add-web-api-controllers"></a>Přidat Kontrolerů webové rozhraní API

V této části přidáme řadiče webového rozhraní API, které podporují operace CRUD (vytvářet, číst, aktualizovat a odstranit). V řadičích použije ke komunikaci s vrstva databáze Entity Framework.

Nejprve můžete odstranit soubor Controllers/ValuesController.cs. Tento soubor obsahuje řadič příklad webového rozhraní API, ale nebudete ho potřebovat pro účely tohoto kurzu.

![](part-2/_static/image2.png)

V dalším kroku sestavte projekt. Generování uživatelského rozhraní Web API používá k nalezení třídy modelu, proto potřebuje kompilované sestavení reflexe.

V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat**, pak vyberte **řadič**.

![](part-2/_static/image3.png)

V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte možnost "Web API 2 řadiče s akcemi používající rozhraní Entity Framework". Klikněte na tlačítko **přidat**.

![](part-2/_static/image4.png)

V **přidat kontroler** dialogové okno, postupujte takto:

1. V **třída modelu** rozevíracího seznamu, vyberte `Author` třídy. (Pokud ho uvedené v rozevírací nabídce nevidíte, ujistěte se, že jste vytvořili projekt.)
2. Zkontrolujte "Použít asynchronní akce kontroleru".
3. Ponechejte pole název řadiče jako &quot;AuthorsController&quot;.
4. Klikněte na tlačítko plus (+) vedle tlačítko **třída kontextu dat**.

![](part-2/_static/image5.png)

V **nový kontext dat** dialogové okno, ponechte výchozí název a klikněte na tlačítko **přidat**.

![](part-2/_static/image6.png)

Klikněte na tlačítko **přidat** k dokončení **přidat kontroler** dialogové okno. Dialogové okno přidá dvě třídy do projektu:

- `AuthorsController` Definuje kontroleru webového rozhraní API. Řadičem implementuje rozhraní REST API, který klienti používat k provádění operací CRUD v seznamu autoři.
- `BookServiceContext` Spravuje objekty entity za běhu, včetně vyplnění objekty s daty z databáze, sledování změn a zachování dat pro databázi. Dědí z `DbContext`.

![](part-2/_static/image7.png)

V tomto okamžiku znovu sestavte projekt. Nyní absolvovat stejný postup pro přidání řadič rozhraní API pro `Book` entity. Tentokrát vyberte `Book` pro třídu modelu, vyberte existující `BookServiceContext` třída pro třídy kontextu dat. (Nevytvářejte nový kontext data.) Klikněte na tlačítko **přidat** pro přidání řadiče.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Předchozí](part-1.md)
> [další](part-3.md)
