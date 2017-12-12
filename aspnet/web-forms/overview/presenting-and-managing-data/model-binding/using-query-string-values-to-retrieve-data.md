---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: "Pomocí hodnoty řetězců dotazu pro filtrování dat pomocí vazby modelu a webových formulářů | Microsoft Docs"
author: tfitzmac
description: "Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 2e5328ccda019462163b984da3661f7322b738df
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Hodnoty řetězců dotazu pro filtrování dat pomocí vazby modelu a webové formuláře
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource). Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.
> 
> Tento kurz ukazuje, jak předat hodnotu v řetězci dotazu a použít tuto hodnotu k načtení dat pomocí vazby modelu.
> 
> V tomto kurzu vychází vytvořené v projektu [starší](retrieving-data.md) částí řady.
> 
> Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB. Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013. Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu budete:

1. Přidat novou stránku zobrazíte zaregistrovaná kurzy pro student
2. Načtení zaregistrovaná kurzy pro vybrané student založená na hodnotě v řetězci dotazu
3. Přidání hypertextového odkazu s hodnotou řetězce dotazu v zobrazení mřížky na novou stránku

Kroky v tomto kurzu jsou dost podobné, co jste udělali v dříve [kurzu](sorting-paging-and-filtering-data.md) k filtrování zobrazených studenty na základě výběru uživatele v rozevíracím seznamu. V tomto kurzu jste použili **řízení** atribut v metodě vyberte, chcete-li určit, že hodnota parametru pochází z ovládacího prvku. V tomto kurzu budete používat **řetězce dotazu** atribut v metodě vyberte, chcete-li určit, že hodnota parametru pochází z řetězce dotazu.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Přidat novou stránku pro zobrazení Studentova kurzy

Přidejte nový webový formulář, který používá stránky předlohy Site.master a název stránky **kurzy**.

V **Courses.aspx** soubor, přidejte zobrazení mřížky kurzy pro vybrané student.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Zadejte metodu vyberte

V **Courses.aspx.cs**, přidáte vyberte metodu s názvem zadaným v zobrazení mřížky **metody SelectMethod** vlastnost. V této metodě budete zadejte dotaz pro načtení Studentova kurzy a určit, že parametr pochází z hodnoty řetězce dotazu se stejným názvem jako parametr.

Nejprve je nutné přidat následující **pomocí** příkazy.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Potom si do Courses.aspx.cs přidejte následující kód:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Atribut QueryString znamená, že hodnotu řetězce dotazu s názvem StudentID se automaticky přiřadí k parametr v této metodě.

## <a name="add-hyperlink-with-query-string-value"></a>Přidání hypertextového odkazu s hodnotou řetězce dotazu

V zobrazení mřížky na Students.aspx přidáte pole hypertextový odkaz, který odkazuje na novou stránku kurzy. Hypertextový odkaz bude obsahovat hodnotu řetězce dotazu s id Studentova.

V Students.aspx přidejte následující pole do sloupce zobrazení mřížky pod pole pro celkový počet kredity.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Spusťte aplikaci a Všimněte si, že zobrazení mřížky nyní zahrnuje odkaz kurzy.

![Přidání hypertextového odkazu](using-query-string-values-to-retrieve-data/_static/image1.png)

Když kliknete na jeden z odkazů, uvidíte této Studentova zaregistrovaná kurzy.

![Zobrazit kurzy](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Závěr

V tomto kurzu jste přidali odkaz s hodnotou řetězce dotazu. Tuto hodnotu řetězce dotazu jste použili pro hodnotu parametru metody select.

V dalším [kurzu](adding-business-logic-layer.md), kód se přesune ze souborů kódu do vrstvy obchodní logiky a vrstva přístupu k datům.

>[!div class="step-by-step"]
[Předchozí](integrating-jquery-ui.md)
[další](adding-business-logic-layer.md)
