---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "Vytváření objektů přenos dat (DTOs) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a>Vytváření objektů přenos dat (DTOs)
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

Pravé nyní naše webové rozhraní API zpřístupní entity databáze do klienta. Klient obdrží data, která mapuje přímo do databázové tabulky. Nicméně, který není vždy vhodné. Někdy budete chtít změnit tvar data, která můžete odeslat do klienta. Například můžete chtít:

- Odeberte cyklické odkazy (viz předchozí části).
- Skryjte konkrétní vlastnosti, které klienti nemají co dělat k zobrazení.
- Vynechte některé vlastnosti za účelem snížení velikosti datové části.
- Vyrovnání objekt grafy, které obsahují vnořených objektů, aby je bylo pohodlnější pro klienty.
- Vyhněte se "typu overpost" ohrožení zabezpečení. (Viz [ověření modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) diskuzi o navýšení zveřejňování.)
- Oddělte vrstvě služby z databázové vrstvě.

Pokud chcete dosáhnout, můžete definovat *objekt pro přenos dat* (DTO). DTO je objekt, který definuje, jak budou data odeslány prostřednictvím sítě. Podívejme se, jak funguje s entitou adresáře. Ve složce modely přidejte dvě třídy DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO` Třída obsahuje všechny vlastnosti z adresáře modelu, vyjma toho, že `AuthorName` je řetězec, který bude obsahovat jméno autora. `BookDTO` Třída obsahuje podmnožinu vlastnosti z `BookDetailDTO`.

Potom nahraďte tyto dvě metody GET v `BooksController` třídy s verzemi, které vracejí DTOs. Použijeme LINQ **vyberte** příkaz Převést z adresáře entity do DTOs.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Tady je SQL generované nové `GetBooks` metoda. Uvidíte, že EF překládá LINQ **vyberte** do příkazu SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Nakonec upravte `PostBook` metoda vrátí DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> V tomto kurzu jsme se převodu na DTOs ručně v kódu. Další možností je použití knihovny jako [AutoMapper](http://automapper.org/) , která zpracovává převod automaticky.

>[!div class="step-by-step"]
[Předchozí](part-4.md)
[další](part-6.md)
