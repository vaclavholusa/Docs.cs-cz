---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Část 2: Vytváření modelů domény | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="c87d6-102">Část 2: Vytváření modelů domény</span><span class="sxs-lookup"><span data-stu-id="c87d6-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="c87d6-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c87d6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c87d6-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="c87d6-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="c87d6-105">Přidat modely</span><span class="sxs-lookup"><span data-stu-id="c87d6-105">Add Models</span></span>

<span data-ttu-id="c87d6-106">Existují tři způsoby, jak přístup Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="c87d6-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="c87d6-107">První databáze: začínat databáze a Entity Framework generuje kód.</span><span class="sxs-lookup"><span data-stu-id="c87d6-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="c87d6-108">První model: začínat visual modelu a Entity Framework vytvoří databáze a kódu.</span><span class="sxs-lookup"><span data-stu-id="c87d6-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="c87d6-109">První kód: začínat kódu a Entity Framework vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="c87d6-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="c87d6-110">Používáme přístup první kódu, proto jsme začněte definováním naše objektů domény jako POCOs (prostý starý CLR objekty).</span><span class="sxs-lookup"><span data-stu-id="c87d6-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="c87d6-111">S tímto přístupem první kódu domény objekty nepotřebujete žádný další kód pro podporu v databázové vrstvě, jako je například transakce nebo stálosti.</span><span class="sxs-lookup"><span data-stu-id="c87d6-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="c87d6-112">(Konkrétně, není nutné dědění z [objektů EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) třídy.) Datových poznámek stále můžete řídit, jak rozhraní Entity Framework vytvoří schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="c87d6-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="c87d6-113">Protože POCOs nepřenášejí žádné další vlastnosti, které popisují [databáze stavu](https://msdn.microsoft.com/library/system.data.entitystate.aspx), může být snadno serializovat na JSON nebo XML.</span><span class="sxs-lookup"><span data-stu-id="c87d6-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="c87d6-114">Ale který neznamená, že by měla vždycky vystavit vaší Entity Framework modely přímo na klienty, jako ukážeme později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c87d6-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="c87d6-115">Vytvoříme POCOs následující:</span><span class="sxs-lookup"><span data-stu-id="c87d6-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="c87d6-116">Produkt</span><span class="sxs-lookup"><span data-stu-id="c87d6-116">Product</span></span>
- <span data-ttu-id="c87d6-117">Pořadí</span><span class="sxs-lookup"><span data-stu-id="c87d6-117">Order</span></span>
- <span data-ttu-id="c87d6-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="c87d6-118">OrderDetail</span></span>

<span data-ttu-id="c87d6-119">Pokud chcete vytvořit jednotlivé třídy, klikněte pravým tlačítkem na složku modely v Průzkumníku řešení.</span><span class="sxs-lookup"><span data-stu-id="c87d6-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="c87d6-120">V místní nabídce vyberte **přidat** a pak vyberte **třídy.**</span><span class="sxs-lookup"><span data-stu-id="c87d6-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="c87d6-121">Přidat `Product` za následující implementaci třídy:</span><span class="sxs-lookup"><span data-stu-id="c87d6-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="c87d6-122">Podle konvence, používá rozhraní Entity Framework `Id` vlastnost jako primární klíč a mapuje na sloupec identity v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="c87d6-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="c87d6-123">Když vytvoříte novou `Product` instance, nebude nastavit hodnotu `Id`, protože databáze generuje hodnotu.</span><span class="sxs-lookup"><span data-stu-id="c87d6-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="c87d6-124">**ScaffoldColumn** technologii ASP.NET MVC tak, aby přeskočil řekne atribut `Id` vlastnost při generování formuláře editor.</span><span class="sxs-lookup"><span data-stu-id="c87d6-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="c87d6-125">**Požadované** atribut slouží k ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="c87d6-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="c87d6-126">Určuje, že `Name` vlastnost musí být neprázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="c87d6-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="c87d6-127">Přidat `Order` třídy:</span><span class="sxs-lookup"><span data-stu-id="c87d6-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="c87d6-128">Přidat `OrderDetail` třídy:</span><span class="sxs-lookup"><span data-stu-id="c87d6-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="c87d6-129">Cizí klíče relace</span><span class="sxs-lookup"><span data-stu-id="c87d6-129">Foreign Key Relations</span></span>

<span data-ttu-id="c87d6-130">Pořadí obsahuje mnoho podrobnosti o pořadí a podrobnosti pro každé pořadí odkazuje na jednoho produktu.</span><span class="sxs-lookup"><span data-stu-id="c87d6-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="c87d6-131">Tyto vztahy představují `OrderDetail` třída definuje vlastnosti s názvem `OrderId` a `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="c87d6-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="c87d6-132">Rozhraní Entity Framework odvodí, že tyto vlastnosti představují cizí klíče a přidá omezení cizího klíče do databáze.</span><span class="sxs-lookup"><span data-stu-id="c87d6-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="c87d6-133">`Order` a `OrderDetail` třídy také zahrnovat "navigační" vlastnosti, které obsahují odkazy na související objekty.</span><span class="sxs-lookup"><span data-stu-id="c87d6-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="c87d6-134">Zadané pořadí, můžete přejít na tyto produkty v pořadí podle navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="c87d6-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="c87d6-135">Kompilace projektu nyní.</span><span class="sxs-lookup"><span data-stu-id="c87d6-135">Compile the project now.</span></span> <span data-ttu-id="c87d6-136">Rozhraní Entity Framework využívá reflexe k vyhledávání vlastností modely, takže vyžaduje kompilované sestavení vytvořit schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="c87d6-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="c87d6-137">Konfigurace formátovací moduly typu média</span><span class="sxs-lookup"><span data-stu-id="c87d6-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="c87d6-138">A [formátovací modul typu média](../../formats-and-model-binding/media-formatters.md) je objekt, který serializuje data při webového rozhraní API zapíše text odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="c87d6-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="c87d6-139">Formátovací moduly vestavěné podporovat výstup JSON a XML.</span><span class="sxs-lookup"><span data-stu-id="c87d6-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="c87d6-140">Ve výchozím nastavení obě tyto formátování serializovat všechny objekty podle hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c87d6-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="c87d6-141">Serializace hodnotou vytvoří problém, pokud obsahuje grafu objektu. cyklické odkazy.</span><span class="sxs-lookup"><span data-stu-id="c87d6-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="c87d6-142">Je to přesně tento případ se `Order` a `OrderDetail` třídy, protože každý obsahuje odkaz na druhý.</span><span class="sxs-lookup"><span data-stu-id="c87d6-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="c87d6-143">Formátovací modul bude použijte odkazy na zápis každého objektu podle hodnoty a přejděte v kroužky.</span><span class="sxs-lookup"><span data-stu-id="c87d6-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="c87d6-144">Proto je potřeba změnit výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="c87d6-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="c87d6-145">V Průzkumníku řešení rozbalte aplikace\_spustit složku a otevřete soubor s názvem WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="c87d6-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="c87d6-146">Přidejte následující kód, který `WebApiConfig` třídy:</span><span class="sxs-lookup"><span data-stu-id="c87d6-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="c87d6-147">Tento kód nastaví formátování JSON pro zachování odkazy na objekty a úplně odebere formátovací modul XML z kanálu.</span><span class="sxs-lookup"><span data-stu-id="c87d6-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="c87d6-148">(Můžete nakonfigurovat formátovací modul XML tak, aby byla zachována odkazy na objekty, ale je trochu další práci a potřebujeme pouze JSON pro tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c87d6-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="c87d6-149">Další informace najdete v tématu [zpracování cyklické odkazy objekt](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="c87d6-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c87d6-150">[Předchozí](using-web-api-with-entity-framework-part-1.md)
[další](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="c87d6-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
