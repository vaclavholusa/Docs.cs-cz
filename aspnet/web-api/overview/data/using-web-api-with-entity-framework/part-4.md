---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Zpracování vztahy entit | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 3c82724739b8ccb7c6b13788a5420af1e61c990b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872686"
---
<a name="handling-entity-relations"></a>Vztahy entit zpracování
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://github.com/MikeWasson/BookService)

Tato část popisuje některé podrobnosti o tom, jak EF načte entit v relaci a jak bude zpracováván cyklické navigační vlastnosti ve třídách modelu. (Tato část poskytuje pozadí znalostní báze a není nutné k dokončení tohoto kurzu. Pokud dáváte přednost, přejděte k [část 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Eager načítání versus opožděného načítání

Při použití EF s relační databázi, je důležité pochopit, jak EF načte související data.

Je také užitečné k zobrazení dotazů SQL, které generuje EF. Trasování SQL, přidejte následující řádek kódu `BookServiceContext` konstruktor:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Pokud odešlete požadavek GET /api/books, vrátí JSON takto:

[!code-console[Main](part-4/samples/sample2.cmd)]

Uvidíte, že vlastnost Autor má hodnotu null, i když kniha obsahuje platnou hodnotu AuthorId. Je to způsobeno EF není načítání související entity autora. Protokol trasování příkazu jazyka SQL potvrdí toto:

[!code-console[Main](part-4/samples/sample3.sql)]

Příkaz SELECT přebírá z tabulky knihy a neodkazuje na autora tabulky.

Pro referenci tady je metoda v `BooksController` třídu, která vrátí seznam knih.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Podívejme se, jak se vrací Autor jako součást JSON data. Existují tři způsoby, jak načíst související data v Entity Framework: přes načítání, opožděného načítání a explicitní načítání. Existují kompromis s každou techniku, proto je důležité pochopit, jak pracují.

### <a name="eager-loading"></a>Přes načítání

S *přes načítání*, EF načte entit v relaci jako součást počáteční databázového dotazu. Pokud chcete provést přes načítání, použijte **System.Data.Entity.Include** metoda rozšíření.

[!code-csharp[Main](part-4/samples/sample5.cs)]

Tato hodnota informuje EF zahrnutí dat Autor v dotazu. Pokud jste tuto změnu provést a spusťte aplikaci, nyní JSON data vypadá takto:

[!code-console[Main](part-4/samples/sample6.cmd)]

Protokol trasování ukazuje, že EF provést na tabulky adresáře a vytvořit spojení.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Opožděného načítání

S opožděného načítání EF automaticky načte související entity, když je přímo odkázat navigační vlastnost pro dané entity. Chcete-li povolit opožděného načítání, proveďte navigační vlastnost virtuální. Například ve třídě adresáře:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Nyní zvažte následující kód:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Pokud opožděného načítání je povoleno, přístup k `Author` vlastnost `books[0]` způsobí, že EF pro dotazování databáze od autora.

Opožděného načítání vyžaduje opakované cesty databáze, protože EF odešle dotaz pokaždé, když ho načte související entity. Obecně platí budete chtít opožděného načítání pro objekty, které můžete serializovat zakázáno. Serializátoru, který se má číst všechny vlastnosti v modelu, který aktivuje načítání související entity. Tady jsou například dotazy SQL při EF serializuje seznamu knih s opožděného načítání povolena. Uvidíte, že EF usnadňuje tři autoři tři samostatné dotazy.

[!code-console[Main](part-4/samples/sample10.sql)]

Jsou stále časy, kdy můžete chtít použít opožděného načítání. Přes načítání může způsobit EF generovat velmi složité spojení. Nebo možná budete muset entit v relaci pro malou podmnožinu dat a opožděného načítání by být efektivnější.

Jedním ze způsobů, aby nedocházelo k problémům serializace je serializovat objekty přenos dat (DTOs) namísto objekty entity. Tento přístup vám zobrazit později v článku.

### <a name="explicit-loading"></a>Explicitní načítání

Explicitní načítání je podobný opožděného načítání, s tím rozdílem, že explicitně získat související data v kódu; není provedena automaticky při přístupu k navigační vlastnosti. Explicitní načítání vám dává větší kontrolu nad tím, kdy se načíst související data, ale vyžaduje další kód. Další informace o explicitní načítání, najdete v části [načítání entit v relaci](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Navigační vlastnosti a cyklické odkazy

Když I definovaná adresáře a vytvořit modely, I na definován navigační vlastnost `Book` třídu pro autor knihy relace, ale I nedefinuje navigační vlastnost opačným směrem.

Co se stane, když přidáte odpovídající navigační vlastnost pro `Author` třída?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Bohužel se tím se vytvoří problém při serializaci modely. Při načítání související data, vytvoří cyklická objekt grafu.

![](part-4/_static/image1.png)

Když se pokusí formátovací modul XML nebo JSON k serializaci grafu, vyvolá výjimku. Dva formátovací moduly throw různé výjimky zprávy. Tady je příklad pro formátování JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Tady je formátovací modul XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Jeden řešením je použití DTOs, které popisují, v další části. Alternativně můžete nakonfigurovat JSON a XML formátovacích modulů pro zpracování cyklů grafu. Další informace najdete v tématu [zpracování cyklické odkazy objekt](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

V tomto kurzu není nutné `Author.Book` navigační vlastnost, takže ho můžete nechat.

> [!div class="step-by-step"]
> [Předchozí](part-3.md)
> [další](part-5.md)
