---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Zpracování relací prvků | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 5d05a2e5d4380a15078317545325bd20fde3f83c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754838"
---
<a name="handling-entity-relations"></a><span data-ttu-id="7570b-102">Ošetření relací prvků</span><span class="sxs-lookup"><span data-stu-id="7570b-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="7570b-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7570b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7570b-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="7570b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="7570b-105">Tato část popisuje některé podrobnosti jak EF načte související entity, a jak zpracovávat kruhový navigačních vlastností ve třídách modelu.</span><span class="sxs-lookup"><span data-stu-id="7570b-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="7570b-106">(Tato část obsahuje základní znalosti a není nutné k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="7570b-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="7570b-107">Pokud dáváte přednost, přejděte k [části 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="7570b-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="7570b-108">Eager načítání a opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="7570b-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="7570b-109">Pokud používáte EF relační databáze, je důležité pochopit, jak EF načte související data.</span><span class="sxs-lookup"><span data-stu-id="7570b-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="7570b-110">Je také užitečné prohlédnout si SQL dotazy, které EF generuje.</span><span class="sxs-lookup"><span data-stu-id="7570b-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="7570b-111">Trasování SQL, přidejte následující řádek kódu, který `BookServiceContext` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="7570b-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="7570b-112">Pokud odešlete požadavek GET /api/books, vrátí JSON podobný tomuto:</span><span class="sxs-lookup"><span data-stu-id="7570b-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="7570b-113">Uvidíte, že vlastnost Autor má hodnotu null, i když knihu obsahuje platné AuthorId.</span><span class="sxs-lookup"><span data-stu-id="7570b-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="7570b-114">Důvodem je, EF načítání souvisejících entit Autor.</span><span class="sxs-lookup"><span data-stu-id="7570b-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="7570b-115">Protokol trasování jazyka SQL potvrdí toto:</span><span class="sxs-lookup"><span data-stu-id="7570b-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="7570b-116">Příkaz SELECT přebírá z tabulky knihy a neodkazuje na tabulce Autor.</span><span class="sxs-lookup"><span data-stu-id="7570b-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="7570b-117">Pro informaci je zde metoda `BooksController` třídu, která vrátí seznam seznamů.</span><span class="sxs-lookup"><span data-stu-id="7570b-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="7570b-118">Podívejme se, jakým způsobem jsme vrátit Autor jako součást JSON data.</span><span class="sxs-lookup"><span data-stu-id="7570b-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="7570b-119">Existují tři způsoby, jak načíst souvisejících dat v Entity Framework: předběžné načítání, opožděné načtení a explicitní načtení.</span><span class="sxs-lookup"><span data-stu-id="7570b-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="7570b-120">Existují kompromisy s každou technikou, takže je důležité pochopit, jak fungují.</span><span class="sxs-lookup"><span data-stu-id="7570b-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="7570b-121">Předběžné načítání</span><span class="sxs-lookup"><span data-stu-id="7570b-121">Eager Loading</span></span>

<span data-ttu-id="7570b-122">S *předběžné načítání*, EF načtení souvisejících entit jako součást počáteční databázového dotazu.</span><span class="sxs-lookup"><span data-stu-id="7570b-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="7570b-123">Předběžné načítání, použijte **System.Data.Entity.Include** – metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="7570b-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="7570b-124">To říká EF zahrnout Autor data v dotazu.</span><span class="sxs-lookup"><span data-stu-id="7570b-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="7570b-125">Pokud jste tuto změnu a spuštění aplikace, nyní JSON data vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="7570b-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="7570b-126">Protokol trasování ukazuje, že EF pracoval spojení na tabulky adresáře a Autor.</span><span class="sxs-lookup"><span data-stu-id="7570b-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="7570b-127">Opožděné načtení</span><span class="sxs-lookup"><span data-stu-id="7570b-127">Lazy Loading</span></span>

<span data-ttu-id="7570b-128">Pomocí opožděné načtení EF automaticky načte související entity při dereferenci navigační vlastnost pro danou entitu.</span><span class="sxs-lookup"><span data-stu-id="7570b-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="7570b-129">Pokud chcete povolit opožděné načtení, ujistěte se, navigační vlastnost virtuální.</span><span class="sxs-lookup"><span data-stu-id="7570b-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="7570b-130">Například ve třídě kniha:</span><span class="sxs-lookup"><span data-stu-id="7570b-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="7570b-131">Teď se podíváme následující kód:</span><span class="sxs-lookup"><span data-stu-id="7570b-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="7570b-132">Pokud je povoleno opožděné načtení, přístup k `Author` vlastnost `books[0]` způsobí, že EF dáte dotaz na databázi pro autora.</span><span class="sxs-lookup"><span data-stu-id="7570b-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="7570b-133">Opožděné načtení vyžaduje více cest databáze, protože EF odešle dotaz pokaždé, když se načte související entity.</span><span class="sxs-lookup"><span data-stu-id="7570b-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="7570b-134">Obecně byste měli opožděné načítání je zakázáno pro objekty, které jste serializovat.</span><span class="sxs-lookup"><span data-stu-id="7570b-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="7570b-135">Serializátoru, který se má číst všechny vlastnosti v modelu, který aktivuje načtení souvisejících entit.</span><span class="sxs-lookup"><span data-stu-id="7570b-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="7570b-136">Tady jsou například dotazy SQL při EF serializuje seznamu knih pomocí opožděné načtení povoleno.</span><span class="sxs-lookup"><span data-stu-id="7570b-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="7570b-137">Uvidíte, že EF provede tři samostatné dotazy pro tři autory.</span><span class="sxs-lookup"><span data-stu-id="7570b-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="7570b-138">Stále existují situace, kdy můžete chtít použít opožděné načtení.</span><span class="sxs-lookup"><span data-stu-id="7570b-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="7570b-139">Předběžné načítání může způsobit EF generovat velmi složité spojení.</span><span class="sxs-lookup"><span data-stu-id="7570b-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="7570b-140">Nebo může být nutné související entity pro malou podmnožinu dat a opožděné načtení by být mnohem efektivnější.</span><span class="sxs-lookup"><span data-stu-id="7570b-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="7570b-141">Jedním ze způsobů, aby nedocházelo k problémům serializace je určená k serializaci objektů pro přenos dat (DTO) namísto objekty entity.</span><span class="sxs-lookup"><span data-stu-id="7570b-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="7570b-142">Ukážeme si tento přístup později v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="7570b-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="7570b-143">Explicitní načtení</span><span class="sxs-lookup"><span data-stu-id="7570b-143">Explicit Loading</span></span>

<span data-ttu-id="7570b-144">Explicitní načtení je podobný opožděné načtení, s tím rozdílem, že explicitně v kódu; získat související data je tomu tak není automaticky při přístupu k vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="7570b-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="7570b-145">Explicitní načtení vám dává větší kontrolu nad tím, kdy se načíst související data, ale vyžaduje další kód.</span><span class="sxs-lookup"><span data-stu-id="7570b-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="7570b-146">Další informace o explicitní načtení najdete v tématu [načtení souvisejících entit](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="7570b-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="7570b-147">Vlastnosti navigace a cyklické odkazy</span><span class="sxs-lookup"><span data-stu-id="7570b-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="7570b-148">Když mám definován modely knihy a Autor, můžu definované vlastnost navigace na `Book` třída vztahu autor knihy, ale I v opačném směru nedefinovala navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="7570b-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="7570b-149">Co se stane, pokud chcete přidat odpovídající navigační vlastnost pro `Author` třídy?</span><span class="sxs-lookup"><span data-stu-id="7570b-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="7570b-150">Bohužel tím se vytvoří problém při serializaci modely.</span><span class="sxs-lookup"><span data-stu-id="7570b-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="7570b-151">Pokud načítáte související data, vytvoří cyklický objekt grafu.</span><span class="sxs-lookup"><span data-stu-id="7570b-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="7570b-152">Formátovací modul XML nebo JSON se pokusí k serializaci grafu, vyvolají výjimku.</span><span class="sxs-lookup"><span data-stu-id="7570b-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="7570b-153">Dva formátovací moduly vyvolat zprávy jinou výjimku.</span><span class="sxs-lookup"><span data-stu-id="7570b-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="7570b-154">Tady je příklad pro formátování JSON:</span><span class="sxs-lookup"><span data-stu-id="7570b-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="7570b-155">Toto je formátovací modul XML:</span><span class="sxs-lookup"><span data-stu-id="7570b-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="7570b-156">Jedním řešením je použití DTO, které popisují, v další části.</span><span class="sxs-lookup"><span data-stu-id="7570b-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="7570b-157">Alternativně můžete nakonfigurovat JSON a XML formátovacích modulů pro zpracování cykly grafu.</span><span class="sxs-lookup"><span data-stu-id="7570b-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="7570b-158">Další informace najdete v tématu [zpracování cyklické odkazy objektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="7570b-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="7570b-159">Pro účely tohoto kurzu není nutné `Author.Book` vlastnost navigace, takže ho můžete nechat navýšení kapacity.</span><span class="sxs-lookup"><span data-stu-id="7570b-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7570b-160">[Předchozí](part-3.md)
> [další](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="7570b-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
