---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Zpracování relací prvků | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 7759b828068d99f9975d56671e427ccf6e94aef6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400950"
---
<a name="handling-entity-relations"></a>Ošetření relací prvků
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](https://github.com/MikeWasson/BookService)

Tato část popisuje některé podrobnosti jak EF načte související entity, a jak zpracovávat kruhový navigačních vlastností ve třídách modelu. (Tato část obsahuje základní znalosti a není nutné k dokončení tohoto kurzu. Pokud dáváte přednost, přejděte k [části 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager načítání a opožděné načtení

Pokud používáte EF relační databáze, je důležité pochopit, jak EF načte související data.

Je také užitečné prohlédnout si SQL dotazy, které EF generuje. Trasování SQL, přidejte následující řádek kódu, který `BookServiceContext` konstruktor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Pokud odešlete požadavek GET /api/books, vrátí JSON podobný tomuto:

[!code-console[Main](part-4/samples/sample2.cmd)]

Uvidíte, že vlastnost Autor má hodnotu null, i když knihu obsahuje platné AuthorId. Důvodem je, EF načítání souvisejících entit Autor. Protokol trasování jazyka SQL potvrdí toto:

[!code-console[Main](part-4/samples/sample3.sql)]

Příkaz SELECT přebírá z tabulky knihy a neodkazuje na tabulce Autor.

Pro informaci je zde metoda `BooksController` třídu, která vrátí seznam seznamů.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Podívejme se, jakým způsobem jsme vrátit Autor jako součást JSON data. Existují tři způsoby, jak načíst souvisejících dat v Entity Framework: předběžné načítání, opožděné načtení a explicitní načtení. Existují kompromisy s každou technikou, takže je důležité pochopit, jak fungují.

### <a name="eager-loading"></a>Předběžné načítání

S *předběžné načítání*, EF načtení souvisejících entit jako součást počáteční databázového dotazu. Předběžné načítání, použijte **System.Data.Entity.Include** – metoda rozšíření.

[!code-csharp[Main](part-4/samples/sample5.cs)]

To říká EF zahrnout Autor data v dotazu. Pokud jste tuto změnu a spuštění aplikace, nyní JSON data vypadá takto:

[!code-console[Main](part-4/samples/sample6.cmd)]

Protokol trasování ukazuje, že EF pracoval spojení na tabulky adresáře a Autor.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Opožděné načtení

Pomocí opožděné načtení EF automaticky načte související entity při dereferenci navigační vlastnost pro danou entitu. Pokud chcete povolit opožděné načtení, ujistěte se, navigační vlastnost virtuální. Například ve třídě kniha:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Teď se podíváme následující kód:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Pokud je povoleno opožděné načtení, přístup k `Author` vlastnost `books[0]` způsobí, že EF dáte dotaz na databázi pro autora.

Opožděné načtení vyžaduje více cest databáze, protože EF odešle dotaz pokaždé, když se načte související entity. Obecně byste měli opožděné načítání je zakázáno pro objekty, které jste serializovat. Serializátoru, který se má číst všechny vlastnosti v modelu, který aktivuje načtení souvisejících entit. Tady jsou například dotazy SQL při EF serializuje seznamu knih pomocí opožděné načtení povoleno. Uvidíte, že EF provede tři samostatné dotazy pro tři autory.

[!code-console[Main](part-4/samples/sample10.sql)]

Stále existují situace, kdy můžete chtít použít opožděné načtení. Předběžné načítání může způsobit EF generovat velmi složité spojení. Nebo může být nutné související entity pro malou podmnožinu dat a opožděné načtení by být mnohem efektivnější.

Jedním ze způsobů, aby nedocházelo k problémům serializace je určená k serializaci objektů pro přenos dat (DTO) namísto objekty entity. Ukážeme si tento přístup později v tomto článku.

### <a name="explicit-loading"></a>Explicitní načtení

Explicitní načtení je podobný opožděné načtení, s tím rozdílem, že explicitně v kódu; získat související data je tomu tak není automaticky při přístupu k vlastnosti navigace. Explicitní načtení vám dává větší kontrolu nad tím, kdy se načíst související data, ale vyžaduje další kód. Další informace o explicitní načtení najdete v tématu [načtení souvisejících entit](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Vlastnosti navigace a cyklické odkazy

Když mám definován modely knihy a Autor, můžu definované vlastnost navigace na `Book` třída vztahu autor knihy, ale I v opačném směru nedefinovala navigační vlastnost.

Co se stane, pokud chcete přidat odpovídající navigační vlastnost pro `Author` třídy?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Bohužel tím se vytvoří problém při serializaci modely. Pokud načítáte související data, vytvoří cyklický objekt grafu.

![](part-4/_static/image1.png)

Formátovací modul XML nebo JSON se pokusí k serializaci grafu, vyvolají výjimku. Dva formátovací moduly vyvolat zprávy jinou výjimku. Tady je příklad pro formátování JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Toto je formátovací modul XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Jedním řešením je použití DTO, které popisují, v další části. Alternativně můžete nakonfigurovat JSON a XML formátovacích modulů pro zpracování cykly grafu. Další informace najdete v tématu [zpracování cyklické odkazy objektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Pro účely tohoto kurzu není nutné `Author.Book` vlastnost navigace, takže ho můžete nechat navýšení kapacity.

> [!div class="step-by-step"]
> [Předchozí](part-3.md)
> [další](part-5.md)
