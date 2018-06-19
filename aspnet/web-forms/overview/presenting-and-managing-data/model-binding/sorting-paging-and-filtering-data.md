---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Řazení, stránkování a filtrování dat pomocí vazby modelu a webové formuláře | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885403"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Řazení, stránkování a filtrování dat pomocí vazby modelu a webové formuláře
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource). Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.
> 
> Tento kurz ukazuje, jak přidat řazení, stránkování a filtrování dat pomocí vazby modelu.
> 
> V tomto kurzu vychází projektu vytvořeného v prvním [část](retrieving-data.md) řady.
> 
> Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB. Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013. Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu budete:

1. Povolit řazení a stránkování dat
2. Povolit filtrování dat na základě výběru uživatelem.

## <a name="add-sorting"></a>Přidat řazení

Povolení řazení v GridView je velmi snadné. V souboru Student.aspx jednoduše nastavit **AllowSorting** k **true** v GridView. Není nutné nastavovat **SortExpression** hodnotu pro každý sloupec, jako DataField automaticky použije. GridView upravuje dotazu zahrnout uspořádání dat podle vybrané hodnoty. Následující zvýrazněný kód ukazuje přidání, že budete muset udělat umožňující řazení.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Spuštění webové aplikace a testování řazení záznamů student podle hodnot v různé sloupce.

![studenti, kteří řazení](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Přidat stránkování

Povolení stránkování je také velmi snadné. V GridView, nastavte **AllowPaging** vlastnost **true** a nastavte **PageSize** vlastnost počet záznamů, které chcete zobrazit na každé stránce. V tomto kurzu můžete ho nastavit na 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Spusťte webovou aplikaci a Všimněte si, že teď záznamy dělí přes více stránek více než 4 záznamy, které jsou zobrazeny na jedné stránce.

![Přidat stránkování](sorting-paging-and-filtering-data/_static/image4.png)

Při provádění dotazu odložené ke zlepšení efektivity aplikace. Místo načítání celá sada dat, upraví GridView dotaz pro načtení záznamů pouze pro aktuální stránku.

## <a name="filter-records-by-user-selection"></a>Filtrování záznamů pomocí výběr uživatele

Vazby modelu přidá několik atributů, které vám umožní určit, jak nastavit hodnotu pro parametr v metodu vazby modelu. Tyto atributy jsou v **System.Web.ModelBinding** oboru názvů. Mezi ně patří:

- Ovládací prvek
- Soubor cookie
- Formulář
- Profil
- Řetězce dotazu
- RouteData
- Relace
- UserProfile
- Stav zobrazení

V tomto kurzu použijete hodnotu ovládacího prvku pro filtrování záznamů, které se zobrazují v GridView. Přidáte **řízení** atribut metodu dotazu, který jste vytvořili dříve. V [později](using-query-string-values-to-retrieve-data.md) kurzu použijete **řetězce dotazu** atribut parametr k určení, že hodnota parametru pochází z hodnotu řetězce dotazu.

Nejprve přidejte výše ValidationSummary, rozevírací seznam pro filtrování, která studenti, kteří jsou zobrazeny.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

V souboru kódu změňte vyberte metodu získat hodnotu z ovládacího prvku a nastavte název parametru k názvu ovládací prvek, který obsahuje hodnotu.

Je nutné přidat **pomocí** příkaz pro **System.Web.ModelBinding** obor názvů přeložit atribut ovládacího prvku.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Následující kód ukazuje znovu fungovala vyfiltrujete vrácená data na základě hodnoty z rozevíracího seznamu vyberte metodu. Přidávání atribut ovládacího prvku před parametr určuje, že hodnota pro tento parametr pochází z ovládacího prvku se stejným názvem.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Spuštění webové aplikace a vyberte z rozevíracího seznamu pro filtrování seznamu studenty různé hodnoty.

![studenti, kteří filtru](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Závěr

V tomto kurzu jste povolili, řazení a stránkování data. Můžete také povolit, filtrování dat podle hodnoty ovládacího prvku.

V dalším [kurzu](integrating-jquery-ui.md) uživatelského rozhraní se zlepšila integrací widget uživatelského rozhraní JQuery do šablony dynamická data.

> [!div class="step-by-step"]
> [Předchozí](updating-deleting-and-creating-data.md)
> [další](integrating-jquery-ui.md)
