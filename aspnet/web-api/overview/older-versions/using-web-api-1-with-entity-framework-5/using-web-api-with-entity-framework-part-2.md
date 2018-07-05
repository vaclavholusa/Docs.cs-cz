---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Část 2: Vytvoření doménových modelů | Dokumentace Microsoftu'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379722"
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="ef3ed-102">Část 2: Vytvoření doménových modelů</span><span class="sxs-lookup"><span data-stu-id="ef3ed-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="ef3ed-103">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef3ed-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ef3ed-104">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="ef3ed-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="ef3ed-105">Přidání modelů</span><span class="sxs-lookup"><span data-stu-id="ef3ed-105">Add Models</span></span>

<span data-ttu-id="ef3ed-106">Existují tři způsoby, jak přístup Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="ef3ed-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="ef3ed-107">Databáze: začnete s databází a Entity Framework generuje kód.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="ef3ed-108">První model: začínat visual modelu a Entity Framework generuje v databázi a kódu.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="ef3ed-109">Založeno na kódu: spustit s kódem a Entity Framework generuje v databázi.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="ef3ed-110">Používáme přístup založeno na kódu, proto začneme tak, že definujete naše objektů domény jako POCOs (prostý staré CLR objekty).</span><span class="sxs-lookup"><span data-stu-id="ef3ed-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="ef3ed-111">S přístupem založeno na kódu domény objekty nemusí nějaký zvláštní kód pro podporu vrstva databáze, jako je například transakce nebo trvalost.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="ef3ed-112">(Konkrétně, není nutné dědění z [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) třídy.) Datové poznámky můžete stále řídit způsob, jak vytvořit schéma databáze v Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="ef3ed-113">Protože POCOs nemají žádné další vlastnosti, které popisují [databáze stavu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), můžete je snadno serializovat do formátu JSON nebo XML.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="ef3ed-114">Však neznamená, že je potřeba vždy zveřejnit své modely Entity Framework přímo na klienty, jak uvidíme dále v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="ef3ed-115">Vytvoříme POCOs následující:</span><span class="sxs-lookup"><span data-stu-id="ef3ed-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="ef3ed-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="ef3ed-116">Product</span></span>
- <span data-ttu-id="ef3ed-117">Pořadí</span><span class="sxs-lookup"><span data-stu-id="ef3ed-117">Order</span></span>
- <span data-ttu-id="ef3ed-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="ef3ed-118">OrderDetail</span></span>

<span data-ttu-id="ef3ed-119">K vytvoření každé třídě, klikněte pravým tlačítkem na složku modely v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="ef3ed-120">V místní nabídce vyberte **přidat** a pak vyberte **třídy.**</span><span class="sxs-lookup"><span data-stu-id="ef3ed-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="ef3ed-121">Přidat `Product` třídy následující implementaci:</span><span class="sxs-lookup"><span data-stu-id="ef3ed-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="ef3ed-122">Podle konvence, Entity Framework používá `Id` vlastnost jako primární klíč a mapuje na sloupec identity v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="ef3ed-123">Když vytvoříte nový `Product` instance, nebude nastavena hodnotu `Id`, protože databáze generuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="ef3ed-124">**ScaffoldColumn** atribut oznamuje ASP.NET MVC, přejděte `Id` vlastnosti při generování pomocí editoru formuláře.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="ef3ed-125">**Vyžaduje** atribut se používá k ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="ef3ed-126">Určuje, že `Name` vlastnost musí být neprázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="ef3ed-127">Přidat `Order` třídy:</span><span class="sxs-lookup"><span data-stu-id="ef3ed-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="ef3ed-128">Přidat `OrderDetail` třídy:</span><span class="sxs-lookup"><span data-stu-id="ef3ed-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="ef3ed-129">Vztahy cizího klíče</span><span class="sxs-lookup"><span data-stu-id="ef3ed-129">Foreign Key Relations</span></span>

<span data-ttu-id="ef3ed-130">Pořadí obsahuje mnoho podrobnostmi o objednávce a podrobnosti pro každé pořadí odkazuje na jednoho produktu.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="ef3ed-131">K reprezentaci tyto vztahy `OrderDetail` třídy definuje vlastnosti s názvem `OrderId` a `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="ef3ed-132">Entity Framework odvodí, že tyto vlastnosti představují cizího klíče a přidá omezení cizího klíče k databázi.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="ef3ed-133">`Order` a `OrderDetail` třídy zahrnují také "navigační" vlastnosti, které obsahují odkazy na související objekty.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="ef3ed-134">Zadaný objednávky, můžete přejít na produkty v pořadí podle vlastnosti navigace.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="ef3ed-135">Zkompilujte projekt.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-135">Compile the project now.</span></span> <span data-ttu-id="ef3ed-136">Entity Framework používá reflexi ke zjišťování vlastností modely, takže vyžaduje zkompilovaného sestavení k vytvoření schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="ef3ed-137">Konfigurace formátovací moduly typu médií</span><span class="sxs-lookup"><span data-stu-id="ef3ed-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="ef3ed-138">A [formátovací modul typu média](../../formats-and-model-binding/media-formatters.md) je objekt, který serializuje data, když webové rozhraní API zapíše text odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="ef3ed-139">Předdefinované formátování podporuje výstup JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="ef3ed-140">Ve výchozím nastavení obě tyto formátování serializace všechny objekty podle hodnoty.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="ef3ed-141">Serializace podle hodnoty vytvoří problém, pokud obsahuje grafu objektů cyklické odkazy.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="ef3ed-142">Je to přesně tento případ s `Order` a `OrderDetail` třídy, protože každá obsahuje odkaz na druhý.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="ef3ed-143">Formátovací modul bude postupovat podle odkazů, zápis každého objektu podle hodnoty a přejděte zacyklení.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="ef3ed-144">Proto je třeba změnit výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="ef3ed-145">V Průzkumníku řešení rozbalte aplikace\_spusťte složky a otevřete soubor s názvem WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="ef3ed-146">Přidejte následující kód, který `WebApiConfig` třídy:</span><span class="sxs-lookup"><span data-stu-id="ef3ed-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="ef3ed-147">Tento kód nastaví formátování JSON pro zachování odkazy na objekty a zcela odebere formátovací modul XML z kanálu.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="ef3ed-148">(Můžete nakonfigurovat formátovací modul XML zachovat odkazy na objekty, ale je o něco více práce a potřebujeme jen JSON pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef3ed-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="ef3ed-149">Další informace najdete v tématu [zpracování cyklické odkazy objektu](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="ef3ed-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef3ed-150">[Předchozí](using-web-api-with-entity-framework-part-1.md)
> [další](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="ef3ed-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
