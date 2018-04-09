---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Přidání modelu | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a>Přidání modelu
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

V této části přidáte některé třídy pro správu filmy v databázi. Tyto třídy bude &quot;modelu&quot; součástí aplikace ASP.NET MVC.

Budete používat technologie pro přístup k datům rozhraní .NET Framework označuje jako [Entity Framework](https://docs.microsoft.com/ef/) definovat a pracovat s tyto třídy modelu. Podporuje rozhraní Entity Framework (často označované jako EF) vývoj zlepší názvem *Code First*. Kód nejprve vám umožní vytvořit objekty modelu vytvořením jednoduchého třídy. (Jedná se také označuje jako třídy objektů POCO, z &quot;objekty CLR prostý starý.&quot;) Pak můžete mít databázi vytvořené za chodu ze třídy, která umožňuje velmi vyčištění a rychlý vývoj pracovního postupu. Pokud je nutné nejprve vytvořit databázi, můžete přesto provést tomto kurzu se dozvíte o vývoj aplikací MVC a EF. Potom postupujte podle tní Fizmakens [ASP.NET generování uživatelského rozhraní](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) kurz, který obsahuje první postup databáze.

## <a name="adding-model-classes"></a>Přidání třídy modelu

V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.

![](adding-a-model/_static/image1.png)

Zadejte *třída* název &quot;film&quot;.

Následujících pět vlastnosti, které chcete přidat `Movie` třídy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Použijeme `Movie` třída představující filmy v databázi. Každá instance `Movie` objektu bude odpovídat na řádek v tabulce databáze a každou vlastnost `Movie` třída bude mapovat na sloupec v tabulce.

Poznámka: Aby bylo možné používat System.Data.Entity a související třídy, musíte nainstalovat [balíček NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/). Pomocí následujícího odkazu pro další pokyny.

Do stejného souboru přidejte následující `MovieDBContext` třídy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Třída reprezentuje kontext Entity Framework film databáze, která zpracovává načítání, ukládání a aktualizace `Movie` třídy instance v databázi. `MovieDBContext` Je odvozena z `DbContext` základní třída poskytuje rozhraní Entity Framework.

Aby bylo možné odkazovat `DbContext` a `DbSet`, je nutné přidat následující `using` příkaz v horní části souboru:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

To provedete tak, že ručně přidáte na pomocí příkazu, nebo můžete pozastavte ukazatel myši nad červenými vlnovkami, klikněte na `Show potential fixes` a klikněte na tlačítko `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Poznámka: Několik nepoužívané `using` příkazy byly odebrány. Visual Studio zobrazí šedě nepoužité závislé součásti. Můžete odebrat nepoužité závislé součásti podržením ukazatele nad šedé závislosti, klikněte na tlačítko `Show potential fixes` a klikněte na tlačítko **odebrat nepoužité řetězce.**

![](adding-a-model/_static/image3.png)

Přidali jsme nakonec model (M v MVC). V další části budete pracovat s připojovací řetězec databáze.

> [!div class="step-by-step"]
> [Předchozí](adding-a-view.md)
> [další](creating-a-connection-string.md)
