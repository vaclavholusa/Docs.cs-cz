---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Část 2: Vytváření modelů domény | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84631494c1be266c21e5e5702182df717b1d29b0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878578"
---
<a name="part-2-creating-the-domain-models"></a>Část 2: Vytváření modelů domény
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Přidat modely

Existují tři způsoby, jak přístup Entity Framework:

- První databáze: začínat databáze a Entity Framework generuje kód.
- První model: začínat visual modelu a Entity Framework vytvoří databáze a kódu.
- První kód: začínat kódu a Entity Framework vytvoří databázi.

Používáme přístup první kódu, proto jsme začněte definováním naše objektů domény jako POCOs (prostý starý CLR objekty). S tímto přístupem první kódu domény objekty nepotřebujete žádný další kód pro podporu v databázové vrstvě, jako je například transakce nebo stálosti. (Konkrétně, není nutné dědění z [objektů EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) třídy.) Datových poznámek stále můžete řídit, jak rozhraní Entity Framework vytvoří schématu databáze.

Protože POCOs nepřenášejí žádné další vlastnosti, které popisují [databáze stavu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), může být snadno serializovat na JSON nebo XML. Ale který neznamená, že by měla vždycky vystavit vaší Entity Framework modely přímo na klienty, jako ukážeme později v tomto kurzu.

Vytvoříme POCOs následující:

- Produkt
- Pořadí
- OrderDetail

Pokud chcete vytvořit jednotlivé třídy, klikněte pravým tlačítkem na složku modely v Průzkumníku řešení. V místní nabídce vyberte **přidat** a pak vyberte **třídy.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Přidat `Product` za následující implementaci třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Podle konvence, používá rozhraní Entity Framework `Id` vlastnost jako primární klíč a mapuje na sloupec identity v tabulce databáze. Když vytvoříte novou `Product` instance, nebude nastavit hodnotu `Id`, protože databáze generuje hodnotu.

**ScaffoldColumn** technologii ASP.NET MVC tak, aby přeskočil řekne atribut `Id` vlastnost při generování formuláře editor. **Požadované** atribut slouží k ověření modelu. Určuje, že `Name` vlastnost musí být neprázdný řetězec.

Přidat `Order` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Přidat `OrderDetail` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Cizí klíče relace

Pořadí obsahuje mnoho podrobnosti o pořadí a podrobnosti pro každé pořadí odkazuje na jednoho produktu. Tyto vztahy představují `OrderDetail` třída definuje vlastnosti s názvem `OrderId` a `ProductId`. Rozhraní Entity Framework odvodí, že tyto vlastnosti představují cizí klíče a přidá omezení cizího klíče do databáze.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

`Order` a `OrderDetail` třídy také zahrnovat "navigační" vlastnosti, které obsahují odkazy na související objekty. Zadané pořadí, můžete přejít na tyto produkty v pořadí podle navigační vlastnosti.

Kompilace projektu nyní. Rozhraní Entity Framework využívá reflexe k vyhledávání vlastností modely, takže vyžaduje kompilované sestavení vytvořit schéma databáze.

## <a name="configure-the-media-type-formatters"></a>Konfigurace formátovací moduly typu média

A [formátovací modul typu média](../../formats-and-model-binding/media-formatters.md) je objekt, který serializuje data při webového rozhraní API zapíše text odpovědi HTTP. Formátovací moduly vestavěné podporovat výstup JSON a XML. Ve výchozím nastavení obě tyto formátování serializovat všechny objekty podle hodnoty.

Serializace hodnotou vytvoří problém, pokud obsahuje grafu objektu. cyklické odkazy. Je to přesně tento případ se `Order` a `OrderDetail` třídy, protože každý obsahuje odkaz na druhý. Formátovací modul bude použijte odkazy na zápis každého objektu podle hodnoty a přejděte v kroužky. Proto je potřeba změnit výchozí chování.

V Průzkumníku řešení rozbalte aplikace\_spustit složku a otevřete soubor s názvem WebApiConfig.cs. Přidejte následující kód, který `WebApiConfig` třídy:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Tento kód nastaví formátování JSON pro zachování odkazy na objekty a úplně odebere formátovací modul XML z kanálu. (Můžete nakonfigurovat formátovací modul XML tak, aby byla zachována odkazy na objekty, ale je trochu další práci a potřebujeme pouze JSON pro tuto aplikaci. Další informace najdete v tématu [zpracování cyklické odkazy objekt](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Předchozí](using-web-api-with-entity-framework-part-1.md)
> [další](using-web-api-with-entity-framework-part-3.md)
