---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Přidání modelu | Microsoft Docs
author: Rick-Anderson
description: 'Poznámka: Aktualizovanou verzi tohoto kurzu je k dispozici, která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, mnohem jednodušší a postupujte podle ukázku...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871958"
---
<a name="adding-a-model"></a>Přidání modelu
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.


V této části přidáte některé třídy pro správu filmy v databázi. Tyto třídy bude &quot;modelu&quot; součástí aplikace ASP.NET MVC.

Budete používat technologie pro přístup k datům rozhraní .NET Framework označuje jako [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) definovat a pracovat s tyto třídy modelu. Podporuje rozhraní Entity Framework (často označované jako EF) vývoj zlepší názvem *Code First*. Kód nejprve vám umožní vytvořit objekty modelu vytvořením jednoduchého třídy. (Jedná se také označuje jako třídy objektů POCO, z &quot;objekty CLR prostý starý.&quot;) Pak můžete mít databázi vytvořené za chodu ze třídy, která umožňuje velmi vyčištění a rychlý vývoj pracovního postupu.

## <a name="adding-model-classes"></a>Přidání třídy modelu

V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.

![](adding-a-model/_static/image1.png)

Zadejte *třída* název &quot;film&quot;.

Následujících pět vlastnosti, které chcete přidat `Movie` třídy:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Použijeme `Movie` třída představující filmy v databázi. Každá instance `Movie` objektu bude odpovídat na řádek v tabulce databáze a každou vlastnost `Movie` třída bude mapovat na sloupec v tabulce.

Do stejného souboru přidejte následující `MovieDBContext` třídy:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Třída reprezentuje kontext Entity Framework film databáze, která zpracovává načítání, ukládání a aktualizace `Movie` třídy instance v databázi. `MovieDBContext` Je odvozena z `DbContext` základní třída poskytuje rozhraní Entity Framework.

Aby bylo možné odkazovat `DbContext` a `DbSet`, je nutné přidat následující `using` příkaz v horní části souboru:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Kompletní *Movie.cs* souboru je uveden níže. (Několik pomocí příkazů, které nejsou potřebné byly odebrány.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Vytvoření připojovacího řetězce a práci s LocalDB serveru SQL

`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi. Jeden otázku může, ale je uveden postup určit se připojí k databázi. Můžete to udělat tak, že přidáte informace o připojení v *Web.config* souboru aplikace.

Otevřete kořenový adresář aplikace *Web.config* souboru. (Není *Web.config* v soubor *zobrazení* složky.) Otevřete *Web.config* souboru označeno červeně.

![](adding-a-model/_static/image2.png)

Přidejte následující připojovací řetězec k `<connectionStrings>` element v *Web.config* souboru.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Následující příklad ukazuje část *Web.config* soubor s přidat nový připojovací řetězec:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Malé množství kódu a XML je všechno, co potřebujete k zápisu, aby bylo možné představují a ukládat data film v databázi.

Dále budete vytvářet nové `MoviesController` třídu, která můžete použít k zobrazení dat film a povolit uživatelům vytvářet nové výpisy film.

> [!div class="step-by-step"]
> [Předchozí](adding-a-view.md)
> [další](accessing-your-models-data-from-a-controller.md)
