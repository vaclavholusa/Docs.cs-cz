---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: "Přidání modelu (VB) | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>Přidání modelu (VB)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se VB.NET zdrojový kód je k dispozici v tomto tématu. [Stáhnout verzi VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost C#, přepnout [C# verze](../cs/adding-a-model.md) tohoto kurzu.


## <a name="adding-a-model"></a>Přidání modelu

V této části přidáte některé třídy pro správu filmy v databázi. Tyto třídy bude část "model" aplikace ASP.NET MVC.

Technologie pro přístup k datům rozhraní .NET Framework označuje jako rozhraní Entity Framework budete používat k definování a pracovat s tyto třídy modelu. Podporuje rozhraní Entity Framework (často označované jako EF) vývoj zlepší názvem *Code First*. Kód nejprve vám umožní vytvořit objekty modelu vytvořením jednoduchého třídy. (Jedná se také označují jako tříd objektů POCO, z "prostý starý objekty CLR".) Pak můžete mít databázi vytvořené za chodu ze třídy, která umožňuje velmi vyčištění a rychlý vývoj pracovního postupu.

## <a name="adding-model-classes"></a>Přidání třídy modelu

V **Průzkumníku řešení**, klikněte pravým tlačítkem *modely* složky, vyberte **přidat**a potom vyberte **třída**.

![](adding-a-model/_static/image1.png)

Název třídy "Film".

Následujících pět vlastnosti, které chcete přidat `Movie` třídy:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Použijeme `Movie` třída představující filmy v databázi. Každá instance `Movie` objektu bude odpovídat na řádek v tabulce databáze a každou vlastnost `Movie` třída bude mapovat na sloupec v tabulce.

Do stejného souboru přidejte následující `MovieDBContext` třídy:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext` Třída reprezentuje kontext Entity Framework film databáze, která zpracovává načítání, ukládání a aktualizace `Movie` třídy instance v databázi. `MovieDBContext` Je odvozena z `DbContext` základní třída poskytuje rozhraní Entity Framework. Další informace o `DbContext` a `DbSet`, najdete v části [produktivitu vylepšení rozhraní Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Aby bylo možné odkazovat `DbContext` a `DbSet`, je nutné přidat následující `imports` příkaz v horní části souboru:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Kompletní *Movie.vb* souboru je uveden níže.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Vytvoření připojovacího řetězce a práci s SQL Server Compact

`MovieDBContext` Třídy, které jste vytvořili zpracovává úlohu s připojením k databázi a mapování `Movie` objekty záznamy v databázi. Jeden otázku může, ale je uveden postup určit se připojí k databázi. Můžete to udělat tak, že přidáte informace o připojení v *Web.config* souboru aplikace.

Otevřete kořenový adresář aplikace *Web.config* souboru. (Není *Web.config* v soubor *zobrazení* složky.) Následující obrázek zobrazit i *Web.config* soubory; otevřete *Web.config* souboru v kroužku červeně.

![](adding-a-model/_static/image2.png)

## 

Přidejte následující připojovací řetězec k `<connectionStrings>` element v *Web.config* souboru.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Následující příklad ukazuje část *Web.config* soubor s přidat nový připojovací řetězec:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Malé množství kódu a XML je všechno, co potřebujete k zápisu, aby bylo možné představují a ukládat data film v databázi.

Dále budete vytvářet nové `MoviesController` třídu, která můžete použít k zobrazení dat film a povolit uživatelům vytvářet nové výpisy film.

>[!div class="step-by-step"]
[Předchozí](adding-a-view.md)
[další](accessing-your-models-data-from-a-controller.md)
