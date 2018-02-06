---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: "Přidání modelu | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 79f136257119a8600a65e8d7c5f6e99cb9abceae
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/05/2018
---
<a name="adding-a-model"></a><span data-ttu-id="27d84-102">Přidání modelu</span><span class="sxs-lookup"><span data-stu-id="27d84-102">Adding a Model</span></span>
====================
<span data-ttu-id="27d84-103">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="27d84-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="27d84-104">V této části přidáte některé třídy pro správu filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="27d84-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="27d84-105">Tyto třídy bude &quot;modelu&quot; součástí aplikace ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27d84-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="27d84-106">Budete používat technologie pro přístup k datům rozhraní .NET Framework označuje jako [Entity Framework](https://docs.microsoft.com/ef/) definovat a pracovat s tyto třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="27d84-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="27d84-107">Podporuje rozhraní Entity Framework (často označované jako EF) vývoj zlepší názvem *Code First*.</span><span class="sxs-lookup"><span data-stu-id="27d84-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="27d84-108">Kód nejprve vám umožní vytvořit objekty modelu vytvořením jednoduchého třídy.</span><span class="sxs-lookup"><span data-stu-id="27d84-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="27d84-109">(Jedná se také označuje jako třídy objektů POCO, z &quot;objekty CLR prostý starý.&quot;) Pak můžete mít databázi vytvořené za chodu ze třídy, která umožňuje velmi vyčištění a rychlý vývoj pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="27d84-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="27d84-110">Pokud je nutné nejprve vytvořit databázi, můžete přesto provést tomto kurzu se dozvíte o vývoj aplikací MVC a EF.</span><span class="sxs-lookup"><span data-stu-id="27d84-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="27d84-111">Potom postupujte podle tní Fizmakens [ASP.NET generování uživatelského rozhraní](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) kurz, který obsahuje první postup databáze.</span><span class="sxs-lookup"><span data-stu-id="27d84-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="27d84-112">Přidání třídy modelu</span><span class="sxs-lookup"><span data-stu-id="27d84-112">Adding Model Classes</span></span>

<span data-ttu-id="27d84-113">V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.</span><span class="sxs-lookup"><span data-stu-id="27d84-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="27d84-114">Zadejte *třída* název &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="27d84-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="27d84-115">Následujících pět vlastnosti, které chcete přidat `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="27d84-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="27d84-116">Použijeme `Movie` třída představující filmy v databázi.</span><span class="sxs-lookup"><span data-stu-id="27d84-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="27d84-117">Každá instance `Movie` objektu bude odpovídat na řádek v tabulce databáze a každou vlastnost `Movie` třída bude mapovat na sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="27d84-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="27d84-118">Poznámka: Aby bylo možné používat System.Data.Entity a související třídy, musíte nainstalovat [balíček NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="27d84-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="27d84-119">Pomocí následujícího odkazu pro další pokyny.</span><span class="sxs-lookup"><span data-stu-id="27d84-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="27d84-120">Do stejného souboru přidejte následující `MovieDBContext` třídy:</span><span class="sxs-lookup"><span data-stu-id="27d84-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="27d84-121">`MovieDBContext` Třída reprezentuje kontext Entity Framework film databáze, která zpracovává načítání, ukládání a aktualizace `Movie` třídy instance v databázi.</span><span class="sxs-lookup"><span data-stu-id="27d84-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="27d84-122">`MovieDBContext` Je odvozena z `DbContext` základní třída poskytuje rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="27d84-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="27d84-123">Aby bylo možné odkazovat `DbContext` a `DbSet`, je nutné přidat následující `using` příkaz v horní části souboru:</span><span class="sxs-lookup"><span data-stu-id="27d84-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="27d84-124">To provedete tak, že ručně přidáte na pomocí příkazu, nebo můžete pozastavte ukazatel myši nad červenými vlnovkami, klikněte na `Show potential fixes` a klikněte na tlačítko`using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="27d84-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="27d84-125">Poznámka: Několik nepoužívané `using` příkazy byly odebrány.</span><span class="sxs-lookup"><span data-stu-id="27d84-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="27d84-126">Visual Studio zobrazí šedě nepoužité závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="27d84-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="27d84-127">Můžete odebrat nepoužité závislé součásti podržením ukazatele nad šedé závislosti, klikněte na tlačítko `Show potential fixes` a klikněte na tlačítko **odebrat nepoužité řetězce.**</span><span class="sxs-lookup"><span data-stu-id="27d84-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="27d84-128">Přidali jsme nakonec model (M v MVC).</span><span class="sxs-lookup"><span data-stu-id="27d84-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="27d84-129">V další části budete pracovat s připojovací řetězec databáze.</span><span class="sxs-lookup"><span data-stu-id="27d84-129">In the next section you'll work with the database connection string.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="27d84-130">[Předchozí](adding-a-view.md)
[další](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="27d84-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
