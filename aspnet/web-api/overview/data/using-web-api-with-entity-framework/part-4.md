---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: "Zpracování vztahy entit | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: 58a9dfb621630f23b37247b96ed3a19a661857f1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="handling-entity-relations"></a><span data-ttu-id="a48d7-102">Vztahy entit zpracování</span><span class="sxs-lookup"><span data-stu-id="a48d7-102">Handling Entity Relations</span></span>
====================
<span data-ttu-id="a48d7-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a48d7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a48d7-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="a48d7-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="a48d7-105">Tato část popisuje některé podrobnosti o tom, jak EF načte entit v relaci a jak bude zpracováván cyklické navigační vlastnosti ve třídách modelu.</span><span class="sxs-lookup"><span data-stu-id="a48d7-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="a48d7-106">(Tato část poskytuje pozadí znalostní báze a není nutné k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a48d7-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="a48d7-107">Pokud dáváte přednost, přejděte k [část 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="a48d7-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="a48d7-108">Eager načítání versus opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="a48d7-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="a48d7-109">Při použití EF s relační databázi, je důležité pochopit, jak EF načte související data.</span><span class="sxs-lookup"><span data-stu-id="a48d7-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="a48d7-110">Je také užitečné k zobrazení dotazů SQL, které generuje EF.</span><span class="sxs-lookup"><span data-stu-id="a48d7-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="a48d7-111">Trasování SQL, přidejte následující řádek kódu `BookServiceContext` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="a48d7-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="a48d7-112">Pokud odešlete požadavek GET /api/books, vrátí JSON takto:</span><span class="sxs-lookup"><span data-stu-id="a48d7-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="a48d7-113">Uvidíte, že vlastnost Autor má hodnotu null, i když kniha obsahuje platnou hodnotu AuthorId.</span><span class="sxs-lookup"><span data-stu-id="a48d7-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="a48d7-114">Je to způsobeno EF není načítání související entity autora.</span><span class="sxs-lookup"><span data-stu-id="a48d7-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="a48d7-115">Protokol trasování příkazu jazyka SQL potvrdí toto:</span><span class="sxs-lookup"><span data-stu-id="a48d7-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="a48d7-116">Příkaz SELECT přebírá z tabulky knihy a neodkazuje na autora tabulky.</span><span class="sxs-lookup"><span data-stu-id="a48d7-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="a48d7-117">Pro referenci tady je metoda v `BooksController` třídu, která vrátí seznam knih.</span><span class="sxs-lookup"><span data-stu-id="a48d7-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="a48d7-118">Podívejme se, jak se vrací Autor jako součást JSON data.</span><span class="sxs-lookup"><span data-stu-id="a48d7-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="a48d7-119">Existují tři způsoby, jak načíst související data v Entity Framework: přes načítání, opožděného načítání a explicitní načítání.</span><span class="sxs-lookup"><span data-stu-id="a48d7-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="a48d7-120">Existují kompromis s každou techniku, proto je důležité pochopit, jak pracují.</span><span class="sxs-lookup"><span data-stu-id="a48d7-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="a48d7-121">Přes načítání</span><span class="sxs-lookup"><span data-stu-id="a48d7-121">Eager Loading</span></span>

<span data-ttu-id="a48d7-122">S *přes načítání*, EF načte entit v relaci jako součást počáteční databázového dotazu.</span><span class="sxs-lookup"><span data-stu-id="a48d7-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="a48d7-123">Pokud chcete provést přes načítání, použijte **System.Data.Entity.Include** metoda rozšíření.</span><span class="sxs-lookup"><span data-stu-id="a48d7-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="a48d7-124">Tato hodnota informuje EF zahrnutí dat Autor v dotazu.</span><span class="sxs-lookup"><span data-stu-id="a48d7-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="a48d7-125">Pokud jste tuto změnu provést a spusťte aplikaci, nyní JSON data vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="a48d7-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="a48d7-126">Protokol trasování ukazuje, že EF provést na tabulky adresáře a vytvořit spojení.</span><span class="sxs-lookup"><span data-stu-id="a48d7-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="a48d7-127">Opožděného načítání</span><span class="sxs-lookup"><span data-stu-id="a48d7-127">Lazy Loading</span></span>

<span data-ttu-id="a48d7-128">S opožděného načítání EF automaticky načte související entity, když je přímo odkázat navigační vlastnost pro dané entity.</span><span class="sxs-lookup"><span data-stu-id="a48d7-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="a48d7-129">Chcete-li povolit opožděného načítání, proveďte navigační vlastnost virtuální.</span><span class="sxs-lookup"><span data-stu-id="a48d7-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="a48d7-130">Například ve třídě adresáře:</span><span class="sxs-lookup"><span data-stu-id="a48d7-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="a48d7-131">Nyní zvažte následující kód:</span><span class="sxs-lookup"><span data-stu-id="a48d7-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="a48d7-132">Pokud opožděného načítání je povoleno, přístup k `Author` vlastnost `books[0]` způsobí, že EF pro dotazování databáze od autora.</span><span class="sxs-lookup"><span data-stu-id="a48d7-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="a48d7-133">Opožděného načítání vyžaduje opakované cesty databáze, protože EF odešle dotaz pokaždé, když ho načte související entity.</span><span class="sxs-lookup"><span data-stu-id="a48d7-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="a48d7-134">Obecně platí budete chtít opožděného načítání pro objekty, které můžete serializovat zakázáno.</span><span class="sxs-lookup"><span data-stu-id="a48d7-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="a48d7-135">Serializátoru, který se má číst všechny vlastnosti v modelu, který aktivuje načítání související entity.</span><span class="sxs-lookup"><span data-stu-id="a48d7-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="a48d7-136">Tady jsou například dotazy SQL při EF serializuje seznamu knih s opožděného načítání povolena.</span><span class="sxs-lookup"><span data-stu-id="a48d7-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="a48d7-137">Uvidíte, že EF usnadňuje tři autoři tři samostatné dotazy.</span><span class="sxs-lookup"><span data-stu-id="a48d7-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="a48d7-138">Jsou stále časy, kdy můžete chtít použít opožděného načítání.</span><span class="sxs-lookup"><span data-stu-id="a48d7-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="a48d7-139">Přes načítání může způsobit EF generovat velmi složité spojení.</span><span class="sxs-lookup"><span data-stu-id="a48d7-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="a48d7-140">Nebo možná budete muset entit v relaci pro malou podmnožinu dat a opožděného načítání by být efektivnější.</span><span class="sxs-lookup"><span data-stu-id="a48d7-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="a48d7-141">Jedním ze způsobů, aby nedocházelo k problémům serializace je serializovat objekty přenos dat (DTOs) namísto objekty entity.</span><span class="sxs-lookup"><span data-stu-id="a48d7-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="a48d7-142">Tento přístup vám zobrazit později v článku.</span><span class="sxs-lookup"><span data-stu-id="a48d7-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="a48d7-143">Explicitní načítání</span><span class="sxs-lookup"><span data-stu-id="a48d7-143">Explicit Loading</span></span>

<span data-ttu-id="a48d7-144">Explicitní načítání je podobný opožděného načítání, s tím rozdílem, že explicitně získat související data v kódu; není provedena automaticky při přístupu k navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="a48d7-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="a48d7-145">Explicitní načítání vám dává větší kontrolu nad tím, kdy se načíst související data, ale vyžaduje další kód.</span><span class="sxs-lookup"><span data-stu-id="a48d7-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="a48d7-146">Další informace o explicitní načítání, najdete v části [načítání entit v relaci](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="a48d7-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="a48d7-147">Navigační vlastnosti a cyklické odkazy</span><span class="sxs-lookup"><span data-stu-id="a48d7-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="a48d7-148">Když I definovaná adresáře a vytvořit modely, I na definován navigační vlastnost `Book` třídu pro autor knihy relace, ale I nedefinuje navigační vlastnost opačným směrem.</span><span class="sxs-lookup"><span data-stu-id="a48d7-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="a48d7-149">Co se stane, když přidáte odpovídající navigační vlastnost pro `Author` třída?</span><span class="sxs-lookup"><span data-stu-id="a48d7-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="a48d7-150">Bohužel se tím se vytvoří problém při serializaci modely.</span><span class="sxs-lookup"><span data-stu-id="a48d7-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="a48d7-151">Při načítání související data, vytvoří cyklická objekt grafu.</span><span class="sxs-lookup"><span data-stu-id="a48d7-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="a48d7-152">Když se pokusí formátovací modul XML nebo JSON k serializaci grafu, vyvolá výjimku.</span><span class="sxs-lookup"><span data-stu-id="a48d7-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="a48d7-153">Dva formátovací moduly throw různé výjimky zprávy.</span><span class="sxs-lookup"><span data-stu-id="a48d7-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="a48d7-154">Tady je příklad pro formátování JSON:</span><span class="sxs-lookup"><span data-stu-id="a48d7-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="a48d7-155">Tady je formátovací modul XML:</span><span class="sxs-lookup"><span data-stu-id="a48d7-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="a48d7-156">Jeden řešením je použití DTOs, které popisují, v další části.</span><span class="sxs-lookup"><span data-stu-id="a48d7-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="a48d7-157">Alternativně můžete nakonfigurovat JSON a XML formátovacích modulů pro zpracování cyklů grafu.</span><span class="sxs-lookup"><span data-stu-id="a48d7-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="a48d7-158">Další informace najdete v tématu [zpracování cyklické odkazy objekt](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="a48d7-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="a48d7-159">V tomto kurzu není nutné `Author.Book` navigační vlastnost, takže ho můžete nechat.</span><span class="sxs-lookup"><span data-stu-id="a48d7-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a48d7-160">[Předchozí](part-3.md)
[další](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="a48d7-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
